# skyTakeOut

## annotion

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotion {

    String value();
}
```

## aspect

```java
@Aspect
@Component
public class MyAnnotionAspect {

    @Pointcut("execution(* com.czp.mapper.*.*(..)) " +
            "&& @annotation(com.czp.annotion.MyAnnotion)")  // 作用路径 && 上述注解
    public void myPointCut() {}


    @Before("myPointCut()")  // 注解值与上述@Pointcut作用的函数名一致
    public void doAspect(JoinPoint joinPoint) throws Exception {

        // 获取 被注解函数的 注解值
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        String value = signature.getMethod().getAnnotation(AutoFill.class).value();

        // 获取 被注解函数的 实参
        Object[] args = joinPoint.getArgs();
        if(args == null || args.length == 0) {
            return;
        }
        Object entity = args[0];

        ///待装入参数
        LocalDateTime now = LocalDateTime.now();
        Long currentId = BaseContext.getCurrentId();

		///通过反射获得setter()方法
        entity.getClass().getMethod(AutoFillConstant.SET_UPDATE_TIME, LocalDateTime.class)
                .invoke(entity, now);
        entity.getClass().getMethod(AutoFillConstant.SET_UPDATE_USER, Long.class)
                .invoke(entity, currentId);

        if(value == OperationType.INSERT) {
            entity.getClass().getMethod(AutoFillConstant.SET_CREATE_TIME, LocalDateTime.class)
                    .invoke(entity, now);
            entity.getClass().getMethod(AutoFillConstant.SET_CREATE_USER, Long.class)
                    .invoke(entity, currentId);
        }

    }
}
```



# mugoBlog

## annotion

```java
/**
 * Request请求次数拦截 自定义注解
 *
 * @author 陌溪
 * @date 2020年3月6日18:58:12
 */
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Documented
@Order(Ordered.HIGHEST_PRECEDENCE)
public @interface RequestLimit {

    /**
     * 允许访问的次数，默认值100
     */
    int amount() default 100;

    /**
     * 时间段，单位为毫秒，默认值一分钟
     */
    long time() default 60000;
}
```

## aspect

```java
/**
 * RequestLimitAspect 请求限制切面实现
 *
 * @author: 陌溪
 * @create: 2020-03-06-19:05
 */
@Aspect
@RefreshScope
@Component
@Slf4j
public class RequestLimitAspect {

    private final String POINT = "execution(* com.moxi.mogublog.web.restapi..*.*(..))";
    @Autowired
    private RedisUtil redisUtil;
    @Autowired
    private RequestLimitConfig requestLimitConfig;

    @Pointcut(POINT)
    public void pointcut() {

    }

    /**
     * 方法前执行
     */
    @Around("pointcut()")
    public Object around(ProceedingJoinPoint point) throws Throwable {

        // 判断是否开启了接口请求限制
        if (requestLimitConfig.getStart()) {
            ServletRequestAttributes attribute = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();

            HttpServletRequest request = attribute.getRequest();

            //获取IP
            String ip = IpUtils.getIpAddr(request);

            //获取请求路径
            String url = request.getRequestURL().toString();

            //获取方法名称
            String methodName = point.getSignature().getName();

            String key = RedisConf.REQUEST_LIMIT + RedisConf.SEGMENTATION + ip + RedisConf.SEGMENTATION + methodName;

            Method currentMethod = AspectUtil.INSTANCE.getMethod(point);

            //查看接口是否有RequestLimit注解，如果没有则按yml的值全局验证
            if (currentMethod.isAnnotationPresent(RequestLimit.class)) {
                //获取注解
                RequestLimit requestLimit = currentMethod.getAnnotation(RequestLimit.class);
                boolean checkResult = checkWithRedis(requestLimit.amount(), requestLimit.time(), key);
                if (checkResult) {
                    log.info("requestLimited," + "[用户ip:{}],[访问地址:{}]超过了限定的次数[{}]次", ip, url, requestLimit.amount());
                    return ResultUtil.result(ECode.REQUEST_OVER_LIMIT, "接口请求过于频繁");
                }
                return point.proceed();
            }
            boolean checkResult = checkWithRedis(requestLimitConfig.getAmount(), requestLimitConfig.getTime(), key);
            if (checkResult) {
                log.info("requestLimited," + "[用户ip:{}],[访问地址:{}]超过了限定的次数[{}]次", ip, url, requestLimitConfig.getAmount());
                return ResultUtil.result(ECode.REQUEST_OVER_LIMIT, "接口请求过于频繁");
            }
        }
        return point.proceed();
    }

    /**
     * 以redis实现请求记录
     *
     * @param amount 请求次数
     * @param time   时间段
     * @param key
     * @return
     */
    private boolean checkWithRedis(int amount, long time, String key) {
        long count = redisUtil.incrBy(key, 1);
        if (count == 1) {
            redisUtil.expire(key, time, TimeUnit.MILLISECONDS);
        }
        if (count <= amount) {
            return false;
        }
        return true;
    }
}
```

