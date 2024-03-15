### 25.Spring的IOC和AOP

- SpringIOC的意思是控制反转，将创建对象和对象管理的过程交给Spring,而减少业务逻辑代码和对象创建和管理之间的耦合度
- IOC底层原理是：xml解析+工厂模式+反射
- SpringAOP的意思是面向切面，利用AOP可以对业务逻辑的各个部分进行隔离，减少业务逻辑各部分之间的耦合度，提高程序可重用性。
- AOP底层有两种动态代理方式(JDK动态代理+CGLIB动态代理)
- 当有接口情况，使用JDK动态代理，创建接口的实现类的代理对象
- 没有接口的情况，使用CGLIB动态代理,创建子类的代理对象
- JDK动态代理使用Proxy类里面的newProxyInstance方法，传入ClassLoader类加载器，要增加的接口数组，实现InvocationHandler,创建代理对象,可以用匿名内部类，或者创建一个新类
- AOP术语
- 连接点：可以被增强的方法,切入点: 真正被增强的方法，增强: 真正增强的逻辑部分，切面：把通知应用到切入点的过程
- AOP一般基于AspectJ实现AOP操作
- SpringBoot开启AspectJ(@EnableAspectJAutoProxy)

### 26.SpringMVC和SpringBoot的区别和联系

- SpringMVC是为了优化Web开发而基于Spring衍生的一个框架，基于ServletAPI的模型视图控制器,SpringMVC的主要作用是将请求和相应进行解耦，将请求和相应与业务逻辑分开来，控制层用于接受请求，返回请求，业务层Service用于写具体业务实现的代码，SpringMVC可以使用拦截器、过滤器对请求进行处理，像登录拦截器，请求日志拦截器等等。
- SpringBoot是Spring框架中的一个快速开发的框架，可以快速开发生产级别的Spring应用程序,SpringBoot可以自动配置大部分的应用程序，减少繁琐的配置工作，像SpringMVC中写中文乱码，视图解析器都做了自动配置，通过Starter来一键配置Web开发所需要的基本依赖，然后可以用@Bean进行组件注入和覆盖，或者在yml格式中进行修改配置,SpringBoot中很多注解可以配置应用程序的各种组件，比如事务管理器，开启AspecJ,指定扫描包路径。
- 可以在用SpringBoot快速搭建应用程序的基础上，快速开发SpringMVC的应用程序，提供了很多自动配置和插件，二者结合使用，效率更高。

### 27.SpringBoot的自动装配原理

https://zhuanlan.zhihu.com/p/626603268

```plain
@EnableAutoConfiguration
点击注解
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}
@AutoConfigurationPackage点击这个注解

@Import({AutoConfigurationPackages.Registrar.class})

 public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
            AutoConfigurationPackages.register(registry, (String[])(new PackageImports(metadata)).getPackageNames().toArray(new String[0]));
        }
new AutoConfigurationPackage.PackageImports.getPackageNames() 发现是主程序所在的包以及下面的子包
@Import({AutoConfigurationImportSelector.class})
getAutoConfigurationEntry->getCandidateConfigurations->SpringFactoriesLoader.loadFactoryNames
点开这个类，发现FACTORIES_RESOURCE_LOCATION静态变量=META-INF/spring.factories就是需要加载的类
```

### 28.SpringBoot 启动流程

![img](https://xxx.xiaobaitiao.icu/2024/202403151504510.png)

[**https://blog.csdn.net/weixin_45433031/article/details/130634595**](https://blog.csdn.net/weixin_45433031/article/details/130634595)https://blog.csdn.net/weixin_45433031/article/details/130634595

### 83.Token可以存哪些地方，分别有哪些优缺点？

1.客户端（前端）：Token 可以直接存储在客户端（如 Web 浏览器、移动 App 等）中，通常以 Cookie、LocalStorage、SessionStorage 等形式存储。客户端在发送请求时，将 Token 附加到请求头、请求参数或请求体中，用于向服务器进行身份验证和授权。

- 优点：客户端存储 Token 简单，无需服务器端维护 Token 的状态信息，适合单页应用、移动 App 等场景。
- 缺点：客户端存储的 Token 可能面临安全风险，如 XSS 攻击、CSRF 攻击等。如果 Token 泄漏，攻击者可能获取到用户的身份信息，并进行恶意操作。

2.服务器端（后端）：Token 可以由服务器生成和管理，通常存储在服务器端的内存、数据库或分布式缓存（如 Redis）中。服务器在生成 Token 时，会将 Token 发送给客户端，并在后续请求中验证 Token 的有效性。

- 优点：服务器端存储 Token 更加安全，减少了 Token 泄漏的风险。服务器可以灵活管理 Token，如设置过期时间、撤销 Token 等。
- 缺点：服务器端存储 Token 需要维护 Token 的状态信息，增加了服务器的存储和管理成本。在分布式系统中，Token 的管理和验证可能需要跨多个服务器进行协调。

3.第三方服务（如 OAuth2、OpenID Connect 等）：Token 可以由第三方服务（如授权服务器、认证服务器等）生成和管理，用于对多个客户端进行身份验证和授权。

- 优点：第三方服务提供了标准化和安全的身份验证/授权方案，减少了开发和维护的工作量，适用于多个应用场景和多个客户端。
- 缺点：依赖第三方服务，可能面临单点故障、性能瓶颈等问题。需要与第三方服务进行集成和配置，增加了系统的复杂性和维护成本。

需要根据具体的应用场景和安全要求来选择合适的 Token 后端方案，并采取相应的安全措施，以保护用户身份信息的安全性。

### 84.关于JWT有哪些了解?

JWT（JSON Web Token）是一种用于在网络应用中传输信息的开放标准，它定义了一种紧凑且自包含的方式，用于在各方之间安全地传输信息。

JWT 主要由三部分组成：头部（Header）、载荷（Payload）和签名（Signature）。

- 头部（Header）：通常由两部分组成，令牌的类型（即 JWT）和签名算法（如 HMAC-SHA256 或 RSA 等）。
- 载荷（Payload）：用于携带实际的信息，如用户身份、权限等。载荷可以包含自定义的声明（Claim），也可以包含一些预定义的声明，如过期时间（exp）、发布时间（iat）、签发人（iss）等。
- 签名（Signature）：由头部、载荷和预先定义的密钥（或公钥）进行签名而生成的，用于验证消息的完整性和来源。

JWT 的作用主要包括身份认证和授权。在身份认证方面，JWT 可以通过在请求中携带 Token，用于验证用户的身份信息，并允许用户在不需要重新登录的情况下访问受保护的资源。在授权方面，JWT 可以在载荷中包含用户的权限信息，用于进行细粒度的访问控制。

JWT 的优点包括：

1. 简洁性和自包含性：JWT 是一种紧凑的编码格式，可以轻松传输和存储。由于 JWT 是自包含的，包含了全部的信息，因此不需要额外的服务器状态存储。
2. 可扩展性和灵活性：JWT 的载荷部分可以包含自定义的声明，从而支持灵活的定制化需求。
3. 基于标准和开放性：JWT 是一种开放标准，得到了广泛的支持和应用，可以与多种编程语言和框架进行集成。

JWT 的缺点包括：

1. 安全性依赖密钥管理：JWT 的签名依赖于密钥的保护，如果密钥泄漏，可能导致令牌被篡改。因此，密钥的安全管理非常重要。
2. 令牌无法撤销：一旦 JWT 被签发，就无法撤销，除非等待 JWT 过期。这意味着在某些情况下，无法即时地取消或撤销用户的访问权限。
3. 令牌大小可能较大：由于 JWT 包含了完整的信息和签名，因此 JWT 的大小可能较大，尤其当包含大量自定义声明时，可能会增加网络传输和存储的成本。

JWT 可以在许多场景使用，包括但不限于以下情况：

1. 身份认证：JWT 可以用于验证用户身份，确保用户在访问受保护的资源时是合法的，从而实现身份认证的功能。例如，在 Web 应用中，用户登录成功后可以通过 JWT 获取一个令牌，然后将该令牌包含在后续的请求中，用于验证用户的身份。
2. 授权控制：JWT 的载荷部分可以包含用户的权限信息，从而实现细粒度的授权控制。例如，在一个多用户的应用中，可以在 JWT 的载荷中包含用户的角色或权限信息，用于限制用户访问不同的资源或执行不同的操作。
3. 单点登录（SSO）：JWT 可以用于实现单点登录，即用户在一次登录后可以在多个不同的应用中共享登录状态，从而避免重复登录的问题。
4. 移动应用认证：JWT 可以作为移动应用与后端服务器之间的认证机制，用于验证移动应用的合法性和用户身份。
5. 微服务架构中的认证和授权：在微服务架构中，各个微服务之间可能需要进行认证和授权，JWT 可以作为一种轻量级的认证和授权机制，用于实现微服务之间的安全通信。

在 Java 中，可以使用许多现有的 JWT 库来实现 JWT 的生成、验证和解析。例如，常用的 Java JWT 库包括 "jjwt"、"Nimbus JOSE + JWT"、"java-jwt" 等。这些库提供了简单易用的 API，可以方便地生成、验证和解析 JWT，并提供了一些配置选项，用于自定义 JWT 的处理逻辑，以满足应用程序的需求。具体使用方式和代码示例如下：

1. 使用 "jjwt" 库生成 JWT：

```plain
javaCopy codeimport io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

// 生成 JWT
String secretKey = "your-secret-key";
String jwt = Jwts.builder()
            .setSubject("user123")
            .signWith(SignatureAlgorithm.HS256, secretKey)
            .compact();
```

1. 使用 "jjwt" 库验证和解析 JWT：

```plain
javaCopy codeimport io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;

// 验证和解析 JWT
String jwt = "your-jwt-token";
String secretKey = "your-secret-key";
Claims claims = Jwts.parser()
                .setSigningKey(secretKey)
                .parseClaimsJws(jwt)
                .getBody();

// 获取载荷中的信息
String subject = claims.getSubject();
```

需要注意的是，生成 JWT 时需要指定一个密钥用于签名，该密钥需要妥善保管，不应该泄漏。在验证和解析 JWT 时，也需要使用相同的密钥进行签名算法的验证，以确保JWT 的完整性和安全性。

优点：

1. 无状态：由于 JWT 是基于 token 的认证机制，不需要在服务器端存储用户的登录状态，因此可以实现无状态的认证和授权，减轻服务器的负担，并且可以更好地适应分布式和扩展性要求。
2. 简单和可扩展：JWT 的结构简单，易于实现和扩展，可以根据实际需求进行自定义的配置和扩展，例如在载荷中添加自定义的信息、设置过期时间等。
3. 跨平台和跨语言：JWT 是一种跨平台和跨语言的认证和授权机制，可以在不同的平台和语言之间使用，例如 Web、移动应用、IoT 设备等。
4. 安全性：JWT 使用签名算法对令牌进行签名，可以防止篡改和伪造，从而保证令牌的安全性。

缺点：

1. 令牌过大：由于 JWT 包含了签名和载荷信息，因此令牌的大小通常较大，可能会增加网络传输和存储的开销。
2. 无法撤销：一旦 JWT 生成后，无法在有效期内撤销或更改令牌的内容，除非等待令牌自动过期。因此，当令牌被盗用或存在安全风险时，需要等待令牌过期或手动撤销密钥来解决问题。
3. 载荷信息可见：虽然 JWT 的载荷部分可以加密，但默认情况下是明文传输的，可能导致敏感信息泄漏的风险。因此，在使用 JWT 时需要注意不要在载荷中存储敏感信息，或者考虑对载荷进行加密。
4. 无法处理会话管理：与传统的基于会话的认证机制不同，JWT 是无状态的，无法直接处理会话管理、会话超时等需求，需要通过其他方式进行处理，例如在令牌中设置过期时间或通过定时刷新令牌等方式。

总的来说，JWT 是一种简单、轻量级且安全的认证和授权机制，适用于无状态的分布式应用场景，特别适合用于移动应用、微服务架构、跨平台和跨语言的应用场景。然而，在使用 JWT 时需要注意保护密钥的安全性，避免在载荷中存储敏感信息，同时也需要考虑到令牌过大、无法撤销和无法处理会话管理等缺点。在实际应用中，需要根据具体的需求和安全要求来选择合适的认证和授权机制，以确保系统的安全性和性能。

使用场景： JWT 可以在各种场景中使用，特别适合以下情况：

1. 跨平台和跨语言应用：由于 JWT 是基于 token 的认证机制，可以在不同的平台和语言之间进行传递和验证，例如在 Web 应用、移动应用和 IoT 设备之间进行认证和授权。
2. 无状态和分布式应用：由于 JWT 是无状态的认证和授权机制，不需要在服务器端存储用户的登录状态，适用于无状态和分布式的应用场景，例如微服务架构、多服务器环境等。
3. 客户端密钥管理：由于 JWT 使用签名算法对令牌进行签名，因此适用于客户端密钥管理的场景，例如移动应用和前端应用等。
4. 需要可扩展性和灵活性：由于 JWT 的结构简单且可扩展，可以根据实际需求进行自定义的配置和扩展，适用于需要灵活性和可扩展性的应用场景。

总的来说，JWT 适用于需要简单、轻量级、无状态且跨平台的认证和授权需求，特别适合在分布式和跨语言的应用中使用。然而，在选择使用 JWT 时，需要根据具体的业务需求、安全要求和性能要求来进行权衡和选择。

### 86.Spring初始化过程

**Spring框架的初始化过程主要分为以下几个步骤：**

1. 加载配置文件：Spring框架会通过配置文件来获取应用程序需要的配置信息，例如Bean的定义、依赖关系、AOP配置等。常见的配置文件包括XML、Java注解或者Java配置类。
2. 创建IOC容器：Spring框架会根据配置文件的信息，创建一个IoC（Inversion of Control）容器，这个容器具有Bean的实例化、依赖注入、AOP的功能。常见的IoC容器包括ApplicationContext和BeanFactory。
3. 实例化Bean：根据配置信息，Spring框架会将配置的Bean进行实例化。如果Bean是单例模式（默认），则会在容器初始化时创建Bean实例；如果Bean是原型模式，则会在每次请求时创建新的Bean实例。
4. 实现依赖注入：当Bean实例化之后，Spring框架会根据配置信息，将对应的依赖注入到Bean中。Spring框架通过反射机制实现依赖注入，根据配置信息中的依赖关系，自动为Bean注入依赖的对象。
5. 处理Bean的生命周期：Spring框架可以管理Bean的整个生命周期，包括初始化、使用、销毁等。在初始化阶段，Spring可以执行一些特殊的处理，例如调用Bean的初始化方法、实现Bean的代理等。在销毁阶段，Spring可以调用Bean的销毁方法，在容器销毁时释放Bean占用的资源。
6. 提供AOP功能：Spring框架提供了AOP（Aspect-Oriented Programming）功能，可以通过配置文件或者注解的方式实现横切关注点的编程。在初始化过程中，Spring会扫描配置的切面和切点，生成代理对象，并将代理对象与原始对象关联起来。
7. 提供其他可选功能：除了基本的初始化过程，Spring还提供了很多其他特性，例如国际化、事件驱动等。在初始化过程中，Spring可以根据配置信息，加载国际化资源，生成事件对象等。

![img](https://xxx.xiaobaitiao.icu/2024/202403151504649.png)

### 87.MyBatis和MP区别和联系

MyBatis和MyBatis Plus是两个与Java持久层开发相关的开源框架，它们有一些区别和联系。

区别：

1. 使用方式：MyBatis是一款基于XML配置文件和SQL语句的ORM框架，需要手动编写SQL语句和映射关系；而MyBatis Plus在MyBatis的基础上进行了封装，提供了更加便捷的API和代码生成器，可以使用代码进行动态SQL构建和数据库操作。
2. 功能扩展：MyBatis仅提供了基本的SQL映射操作，例如增删改查等；而MyBatis Plus在此基础上提供了更多实用的功能，例如分页、逻辑删除、乐观锁、自动填充等。
3. 易用性：MyBatis Plus使用了简化的API和更加智能的提供者查询条件封装，使得开发变得更加简单和高效。
4. 社区活跃度：MyBatis是一个相对成熟和活跃的框架，拥有庞大的开发者社区和丰富的生态系统；MyBatis Plus作为MyBatis的增强版本，因其提供的便利功能和易用性受到了越来越多的开发者喜爱，并逐渐形成了自己的社区和生态系统。
5. Lambda表达式：MyBatis不支持Lambda形式调用,MyBatisPlus支持Lambda形式调用
6. SQL语句：MyBatis所有SQL语句自己写，MP提供基本的CRUD功能，不需要自己编写SQL。
7. MyBatisX: 插件自动生成实体类，mapper,xml文件

联系：

1. 基于MyBatis：MyBatis Plus是基于MyBatis的增强版本，能够完全兼容MyBatis的特性和功能。
2. 共享资源：MyBatis和MyBatis Plus的许多资源可以相互共享和复用，例如数据源、事务管理等。
3. 共同目标：MyBatis和MyBatis Plus旨在提供灵活、高效、易用的持久层开发解决方案，帮助开发者更加便捷地进行数据库操作。

### 98.Bean的生命周期

- Bean生命周期有七步 

（1）通过构造器创建bean实例（无参数构造） 

（2）为bean的属性设置值和对其他bean引用（调用set方法） 

（3）把bean实例传递bean后置处理器的方法postProcessBeforeInitialization 

（4）调用bean的初始化的方法（需要进行配置初始化的方法） 

（5）把bean实例传递bean后置处理器的方法 postProcessAfterInitialization 

（6）bean可以使用了（对象获取到了） 

（7）当容器关闭时候，调用bean的销毁的方法（需要进行配置销毁的方法）

### 99.Bean的作用域

scope:singleton | prototype设置Bean的作用域

 Singleton和prototype区别: 第一 singleton单实例，prototype多实例 第二 设置scope值是singleton时候，加载spring配置文件时候就会创建单实例对象 设置scope值是prototype时候，不是在加载spring配置文件时候创建 对象，在调用 getBean方法时候创建多实例对象  

### 100.Spring如何解决循环依赖问题？

构造函数注入：通过将依赖关系通过构造函数传递，可以解决循环依赖。当两个bean相互依赖时，Spring会优先创建其中一个bean，并将其作为参数传递给另一个bean的构造函数。

setter方法注入：通过setter方法注入依赖关系，同样可以解决循环依赖。当两个bean相互依赖时，Spring会先创建其中一个bean，并将其注入到另一个bean的setter方法中。

使用@Lazy注解：@Lazy注解可以延迟创建bean的实例，从而解决循环依赖问题。当两个bean相互依赖时，可以使用@Lazy注解将其中一个bean的创建延迟到需要使用时再创建。 需要注意的是，循环依赖可能会导致死锁或无限递归的问题，因此在设计应用程序时应尽量避免循环依赖的产生。如果无法避免，可以通过合理的设计和使用上述解决方式来解决循环依赖问题。

### 101.AOP的术语，以及两种动态代理实现方法，以及它们的区别是什么？

1.连接点：哪些方法可以被增强

2.切入点：真正被增强的方法

3.通知：实际增强的逻辑部分称为通知

4.切面：把通知应用到切入点过程

```plain
Spring4版本
(1).正常情况
1.环绕之前通知
2.前置通知Before
3.被增强的方法
4.环绕之后通知
5.After最终通知
6.AfterReturning 后置通知
(2).异常情况
1.环绕之前通知
2.前置通知Before
3.被增强的方法
4.After最终通知
5.AfterThrowing 异常通知
Spring5版本
(1).正常情况
1.环绕之前通知
2.前置通知Before
3.被增强的方法
4.环绕之后通知
5.AfterReturning后置通知
6.After最终通知
(2).异常情况
1.环绕之前通知
2.前置通知Before
3.被增强的方法
4.AfterThrowing异常通知
5.After最终通知
```

JDK动态代理：在有接口的情况下，创建接口的实现类的代理对象。

CGLIB动态代理：在没有接口的情况下，创建子类的代理对象。

JDK动态代理实现：

创建一个增加类,JDKProxy, 调用Proxy.newProxyInstance(JDKProxy.class.getClassLoader(),要增加的接口数组(实现类)，new 实现类的代理对象去implements InvocationHandler 重写invoke方法，method.invoke是真正执行代码的逻辑。

```java
public class JDKProxy {
    public static void main(String[] args) {
        //创建接口实现类的代理对象
        UserDaoImpl userDao = new UserDaoImpl();
        Class[] interfaces = {UserDao.class};
        UserDao dao = (UserDao) Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao));
        int result = dao.add(1, 2);
        System.out.println("result = " + result);
    }
}

class UserDaoProxy implements InvocationHandler {
    //1.把创建的是谁的代理对象，把谁传递过来
    //有参数构造器
    private Object obj;

    public UserDaoProxy(Object obj) {
        this.obj = obj;
    }

    //增强的逻辑
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //方法之前:
        System.out.println("方法之前执行...." + method.getName() + ":传递的参数..." + Arrays.toString(args));


        //被增强的方法执行
        Object res = method.invoke(obj, args);
        //方法之后
        System.out.println("方法之后执行..." + obj);
        return res;
    }
}
```

### 102.SpringBoot与Spring框架的区别？

Spring Boot是Spring框架的一部分，它是一个用于快速构建基于Spring的应用程序的开发工具。Spring框架本身是一个轻量级的开源Java企业应用程序开发框架，用于构建可移植、可伸缩和易于测试的企业级应用程序。

Spring Boot相对于Spring框架来说有以下几个区别：

1. 简化配置：Spring Boot通过自动配置功能，可以根据应用程序的依赖和配置文件自动配置应用程序。因此，开发者不需要手动配置大量的Spring特性和依赖项。
2. 内嵌服务器：Spring Boot可以内置Tomcat、Jetty等服务器，使得构建和运行应用程序变得更加简单。
3. 快速启动：Spring Boot可以快速启动应用程序，减少了开发和部署过程中的等待时间。
4. 简化依赖管理：Spring Boot通过使用Spring的starter模块，简化了对各种第三方库和框架的依赖管理。
5. 自动化管理：Spring Boot可以通过Actuator模块，提供对应用程序的运行时度量、监视和管理功能。

## 牛客面经整理

### 1.Spring事务一般使用什么注解？有了解过事务注解失效的场景吗?

- Spring 事务一般使用 @Transactional注解
- 首先我们主要知道 @ Transactional 注解可以用于哪些地方？
- 当把@Transactional 注解放在类上时，表示所有该类的public方法都配置相同的事务属性信息。
- 作用于方法：当类配置了@Transactional，方法也配置了@Transactional，方法的事务会覆盖类的事务配置信息。
- 作用于接口：不推荐这种使用方法，因为一旦标注在Interface上并且配置了Spring AOP 使用CGLib动态代理，将会导致@Transactional注解失效

**事务有哪些属性？**

propagation属性（事务传播行为）



**propagation 代表事务的传播行为，默认值为 Propagation.REQUIRED，其他的属性信息如下：**

**Propagation.REQUIRED：如果当前存在事务，则加入该事务，如果当前不存在事务，则创建一个新的事务。( 也就是说如果A方法和B方法都添加了注解，在默认传播模式下，A方法内部调用B方法，会把两个方法的事务合并为一个事务 ）**

**Propagation.SUPPORTS：如果当前存在事务，则加入该事务；如果当前不存在事务，则以非事务的方式继续运行。**

**Propagation.MANDATORY：如果当前存在事务，则加入该事务；如果当前不存在事务，则抛出异常。**

**Propagation.REQUIRES_NEW：重新创建一个新的事务，如果当前存在事务，暂停当前的事务。( 当类A中的 a 方法用默认Propagation.REQUIRED模式，类B中的 b方法加上采用 Propagation.REQUIRES_NEW模式，然后在 a 方法中调用 b方法操作数据库，然而 a方法抛出异常后，b方法并没有进行回滚，因为Propagation.REQUIRES_NEW会暂停 a方法的事务 )**

**Propagation.NOT_SUPPORTED：以非事务的方式运行，如果当前存在事务，暂停当前的事务。**

**Propagation.NEVER：以非事务的方式运行，如果当前存在事务，则抛出异常。**

**Propagation.NESTED ：和 Propagation.REQUIRED 效果一样。**



isolation 属性（事务隔离级别）



**isolation ：事务的隔离级别，默认值为 Isolation.DEFAULT。**

**Isolation.DEFAULT：使用底层数据库默认的隔离级别。**

**Isolation.READ_UNCOMMITTED**

**Isolation.READ_COMMITTED**

**Isolation.REPEATABLE_READ**

**Isolation.SERIALIZABLE**



timeout 属性 （事务超时时间）

timeout ：事务的超时时间，默认值为 -1。如果超过该时间限制但事务还没有完成，则自动回滚事务。



readOnly 属性 (是否只读)

readOnly ：指定事务是否为只读事务，默认值为 false；为了忽略那些不需要事务的方法，比如读取数据，可以设置 read-only 为 true。



rollbackFor 属性(指定要回滚的异常)

rollbackFor ：用于指定能够触发事务回滚的异常类型，可以指定多个异常类型。



noRollbackFor属性(指定不回滚的异常)

noRollbackFor：抛出指定的异常类型，不回滚事务，也可以指定多个异常类型。



**事务失效的场景有哪些？**

#### 1、@Transactional 应用在非 public 修饰的方法上

**失效原因：**

![img](https://xxx.xiaobaitiao.icu/2024/202403151504766.png)

因为在Spring AOP 代理时，如上图所示 TransactionInterceptor （事务拦截器）在目标方法执行前后进行拦截，DynamicAdvisedInterceptor（CglibAopProxy 的内部类）的 intercept 方法或 JdkDynamicAopProxy 的 invoke 方法会间接调用
AbstractFallbackTransactionAttributeSource的
computeTransactionAttribute 方法，获取Transactional 注解的事务配置信息。

```plain
protected TransactionAttribute computeTransactionAttribute(Method method,
    Class<?> targetClass) {
        // Don't allow no-public methods as required.
        if (allowPublicMethodsOnly() && !Modifier.isPublic(method.getModifiers())) {
        return null;
}
```

此方法会检查目标方法的修饰符是否为 public，不是 public则不会获取@Transactional 的属性配置信息。

注意：protected、private 修饰的方法上使用 @Transactional 注解，虽然事务无效，但不会有任何报错，这是我们很容犯错的一点。

#### 2、@Transactional 注解属性 propagation 设置错误

这种失效是由于配置错误，若是错误的配置以下三种 propagation，事务将不会发生回滚。



TransactionDefinition.PROPAGATION_SUPPORTS：如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。

TransactionDefinition.PROPAGATION_NOT_SUPPORTED：以非事务方式运行，如果当前存在事务，则把当前事务挂起。

TransactionDefinition.PROPAGATION_NEVER：以非事务方式运行，如果当前存在事务，则抛出异常。

#### 3、@Transactional 注解属性 rollbackFor 设置错误

rollbackFor 可以指定能够触发事务回滚的异常类型。Spring默认抛出了未检查unchecked异常（继承自 RuntimeException 的异常）或者 Error才回滚事务；其他异常不会触发回滚事务。如果在事务中抛出其他类型的异常，但却期望 Spring 能够回滚事务，就需要指定rollbackFor属性。

![img](https://xxx.xiaobaitiao.icu/2024/202403151504791.png)

#### 4、同一个类中方法调用，导致@Transactional失效 

开发中避免不了会对同一个类里面的方法调用，比如有一个类Test，它的一个方法A，A再调用本类的方法B（不论方法B是用public还是private修饰），但方法A没有声明注解事务，而B方法有。则外部调用方法A之后，方法B的事务是不会起作用的。这也是经常犯错误的一个地方。

那为啥会出现这种情况？其实这还是由于使用Spring AOP代理造成的，因为只有当事务方法被当前类以外的代码调用时，才会由Spring生成的代理对象来管理。



```plain
//@Transactional
    @GetMapping("/test")
    private Integer A() throws Exception {
        CityInfoDict cityInfoDict = new CityInfoDict();
        cityInfoDict.setCityName("2");
        /**
         * B 插入字段为 3的数据
         */
        this.insertB();
        /**
         * A 插入字段为 2的数据
         */
        int insert = cityInfoDictMapper.insert(cityInfoDict);

        return insert;
    }

    @Transactional()
    public Integer insertB() throws Exception {
        CityInfoDict cityInfoDict = new CityInfoDict();
        cityInfoDict.setCityName("3");
        cityInfoDict.setParentCityId(3);

        return cityInfoDictMapper.insert(cityInfoDict);
    }
```

#### 5、异常被你的 catch“吃了”导致@Transactional失效

这种情况是最常见的一种@Transactional注解失效场景，



```plain
@Transactional
    private Integer A() throws Exception {
        int insert = 0;
        try {
            CityInfoDict cityInfoDict = new CityInfoDict();
            cityInfoDict.setCityName("2");
            cityInfoDict.setParentCityId(2);
            /**
             * A 插入字段为 2的数据
             */
            insert = cityInfoDictMapper.insert(cityInfoDict);
            /**
             * B 插入字段为 3的数据
             */
            b.insertB();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

如果B方法内部抛了异常，而A方法此时try catch了B方法的异常，那这个事务还能正常回滚吗？

答案：不能！

会抛出异常：



```plain
org.springframework.transaction.UnexpectedRollbackException: Transaction rolled back because it has been marked as rollback-only
```

因为当ServiceB中抛出了一个异常以后，ServiceB标识当前事务需要rollback。但是ServiceA中由于你手动的捕获这个异常并进行处理，ServiceA认为当前事务应该正常commit。此时就出现了前后不一致，也就是因为这样，抛出了前面的

UnexpectedRollbackException异常。

spring的事务是在调用业务方法之前开始的，业务方法执行完毕之后才执行commit or rollback，事务是否执行取决于是否抛出runtime异常。如果抛出runtime exception 并在你的业务方法中没有catch到的话，事务会回滚。

在业务方法中一般不需要catch异常，如果非要catch一定要抛出throw new RuntimeException()，或者注解中指定抛异常类型@Transactional(rollbackFor=Exception.class)，否则会导致事务失效，数据commit造成数据不一致，所以有些时候try catch反倒会画蛇添足。

#### 6、被final、static关键字修饰的类或方法

CGLIB是通过生成目标类子类的方式生成代理类的，被final、static修饰后，那么在它的代理类中，就无法重写该方法，而添加事务功能。

https://zhuanlan.zhihu.com/p/452094574?utm_id=0

### 2.现在有一个外部的jar包、如何将该jar包交给IOC容器进行管理，并且在项目中使用；

1.使用包扫描 @Component +@ Configuration注解

2.使用@Import 进行引入。

3.@Bean 注解，去set注入或者 @Value 注解，获取配置文件的值。

### 3.介绍一下Spring 的钩子接口

https://www.zhankr.net/119335.html

### 4.SpringBoot 缓存之@Cacheable介绍

https://blog.csdn.net/qq_50652600/article/details/122791156

### 5.Spring 怎么知道Bean已经装配完毕了？

InitializingBean接口只有一个方法：afterPropertiesSet()。在Spring框架中，当一个bean的所有属性都已经被设置完毕后，这个方法就会被调用。也就是说，这个bean一旦被初始化，Spring就会调用这个方法。我们可以在bean的所有属性被设置后，进行一些自定义的初始化工作。

DisposableBean接口也只有一个方法：destroy()。当Spring容器关闭并销毁bean时，这个方法就会被调用。我们可以在bean被销毁前，进行一些清理工作。

在Spring框架中，你可以使用ApplicationListener接口或者@EventListener注解来监听bean初始化完毕的事件，并在初始化完毕后执行相应的操作。

1. 使用ApplicationListener接口：

```plain
import org.springframework.context.ApplicationListener;
import org.springframework.context.event.ContextRefreshedEvent;

public class MyApplicationListener implements ApplicationListener<ContextRefreshedEvent> {

    @Override
    public void onApplicationEvent(ContextRefreshedEvent event) {
        // 在bean初始化完毕后执行的操作
        System.out.println("所有bean初始化完毕");
    }
}
```

在上面的示例中，MyApplicationListener实现了ApplicationListener接口，并重写了onApplicationEvent方法，在该方法中可以编写在bean初始化完毕后需要执行的操作。

1. 使用@EventListener注解：

```plain
import org.springframework.context.event.ContextRefreshedEvent;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;

@Component
public class MyEventListener {

    @EventListener
    public void handleContextRefresh(ContextRefreshedEvent event) {
        // 在bean初始化完毕后执行的操作
        System.out.println("所有bean初始化完毕");
    }
}
```

在上面的示例中，MyEventListener类使用了@Component注解，表示它是一个Spring组件，并使用了@EventListener注解来标记handleContextRefresh方法，该方法会在bean初始化完毕后执行。

无论是使用ApplicationListener接口还是@EventListener注解，当Spring容器初始化完毕后，会触发ContextRefreshedEvent事件，你可以在相应的监听器或方法中编写初始化完毕后需要执行的操作。

### 6.怎么实现服务一启动就将mysql数据同步到redis?

[**https://zhuanlan.zhihu.com/p/609445855**](https://zhuanlan.zhihu.com/p/609445855)

**利用CommandLineRunner和ApplicationRunner**

### 7.Spring 的 IOC 用到了 哪些设计模式？

Spring IOC使用了多种设计模式，其中最主要的是依赖注入（Dependency Injection）模式。以下是Spring IOC使用的一些常见设计模式：

1. 依赖注入（Dependency Injection）：在Spring IOC中，通过将对象之间的依赖关系交由IOC容器来处理，实现了松耦合和高度可测试性。
2. 工厂模式（Factory Pattern）：在Spring IOC中，BeanFactory就是一个工厂类，用于创建和管理Bean实例。
3. 单例模式（Singleton Pattern）：在Spring IOC中，默认情况下所有Bean都是单例的，并且可以通过配置控制是否启用单例模式。
4. 模板方法模式（Template Method Pattern）：在Spring AOP中，通过定义抽象切面类和具体实现类来实现切面逻辑复用。
5. 观察者模式（Observer Pattern）：在Spring事件机制中，观察者监听某个事件源发生的事件，并进行相应的处理。
6. 策略模式（Strategy Pattern）：在Spring MVC中，通过定义多个HandlerMapping策略来实现URL到Controller映射。

### 8.Spring的管理事务的方式有几种？

- **编程式事务：在代码中硬编码(不推荐使用) : 通过 TransactionTemplate的execute或者 TransactionManager的commit和rollback 手动管理事务，实际应用中很少使用，但是对于你理解 Spring 事务管理原理有帮助。**
- **声明式事务：在 XML 配置文件中配置或者直接基于注解（推荐使用） : 实际是通过 AOP 实现（基于@Transactional 的全注解方式使用最多）**

**9.Spring的容器解析**

https://zhuanlan.zhihu.com/p/74832770

### 10.MySQL 的动态SQL了解哪些？

![img](https://xxx.xiaobaitiao.icu/2024/202403151504473.png)

![img](https://xxx.xiaobaitiao.icu/2024/202403151504520.png)

https://segmentfault.com/a/1190000039335704#item-3

https://cloud.tencent.com/developer/article/1943349

### 11.MyBatis缓存介绍一下？

- **MyBatis**的一级缓存默认开启，且默认作用范围为**SESSION**，即一级缓存在一个会话中生效，也可以通过配置将作用范围设置为**STATEMENT**，让一级缓存仅针对当前执行的**SQL**语句生效；
- 在同一个会话中，执行**增**，**删**，**改**操作会使本会话中的一级缓存失效；
- 不同会话持有不同的一级缓存，本会话内的操作不会影响其它会话内的一级缓存。



- **MyBatis**中的二级缓存默认开启，可以在**MyBatis**配置文件中的<**settings**>中添加<**setting name="cacheEnabled" value="false"/**>将二级缓存关闭；
- **MyBatis**中的二级缓存作用范围是同一命名空间下的多个会话共享，这里的命名空间就是映射文件的**namespace**，即不同会话使用同一映射文件中的**SQL**语句对数据库执行操作并提交事务后，均会影响这个映射文件持有的二级缓存；
- **MyBatis**中执行查询操作后，需要提交事务才能将查询结果缓存到二级缓存中；
- **MyBatis**中执行增，删或改操作并提交事务后，会清空对应的二级缓存；
- **MyBatis**中需要在映射文件中添加<**cache**>标签来为映射文件配置二级缓存，也可以在映射文件中添加<**cache-ref**>标签来引用其它映射文件的二级缓存以达到多个映射文件持有同一份二级缓存的效果。

![img](https://xxx.xiaobaitiao.icu/2024/202403151504919.png)

![img](https://xxx.xiaobaitiao.icu/2024/202403151504397.png)

![img](https://xxx.xiaobaitiao.icu/2024/202403151504902.png)

https://juejin.cn/post/7206261082453671994?searchId=20231130183932536A2DF70F3F70AB3C93



### 12.Spring如何解决循环依赖问题?三级缓存都介绍一下？

![img](https://xxx.xiaobaitiao.icu/2024/202403151504543.png)

![img](https://xxx.xiaobaitiao.icu/2024/202403151504549.png)

![img](https://xxx.xiaobaitiao.icu/2024/202403151504860.png)

![img](https://xxx.xiaobaitiao.icu/2024/202403151504062.png)

https://blog.csdn.net/weixin_44102992/article/details/128106055



### 13.Spring 中使用工厂模式 BeanFactory 和 ApplicationContext 接口

BeanFactory：延迟注入，等到某个 Bean 使用的时候才会注入，因此如果某个属性没有注入，等到 getBean 方法调用，才会抛出异常。

ApplicationContext：一次性创建所有 Bean，而且有预先检验机制，有利于检查所依赖属性是否注入，缺点就是当 Bean 配置较多时，程序启动较慢。

1）利用 MessageSource 支持国际化

2）可以向注册为监听器的 Bean 发送事件，利用 ApplicationEvent 和 ApplicationListener。

**ApplicationContext 的三个实现类：**

1. ClassPathXmlApplication：把上下文文件当成类路径资源。
2. FileSystemXmlApplication：从文件系统中的 XML 文件载入上下文定义信息。
3. XmlWebApplicationContext：从 Web 系统中的 XML 文件载入上下文定义信息。



### 14.Spring 中使用单例设计模式 Bean 的作用域和单例注册表

**使用单例模式的好处**

1）对于频繁使用的对象，可以极大减少创建对象所花费的时间、

2）new 操作次数较少，对于系统内存使用频率降低，减轻 GC 压力，缩短 GC 停顿时间。

**Spring 中 bean 的默认作用域就是 singleton(单例)的。** 除了 singleton 作用域，Spring 中 bean 还有下面几种作用域：

- **prototype** : 每次获取都会创建一个新的 bean 实例。也就是说，连续 getBean() 两次，得到的是不同的 Bean 实例。
- **request** （仅 Web 应用可用）: 每一次 HTTP 请求都会产生一个新的 bean（请求 bean），该 bean 仅在当前 HTTP request 内有效。
- **session** （仅 Web 应用可用） : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean（会话 bean），该 bean 仅在当前 HTTP session 内有效。
- **application/global-session** （仅 Web 应用可用）：每个 Web 应用在启动时创建一个 Bean（应用 Bean），，该 bean 仅在当前应用启动时间内有效。
- **websocket** （仅 Web 应用可用）：每一次 WebSocket 会话产生一个新的 bean。

Spring 通过 ConcurrentHashMap 实现单例注册表的特殊方式实现单例模式。

过程：

1）用断言检查 getSingleton 的Bean 昵称是否为空

2）用 Synchronized 关键字锁住单例注册表，先从注册表（缓存）中检查是否存在实例，如果缓存中有，直接返回，如果缓存中没有，从单例工厂中获得该 Bean 实例，放入单例注册表,以 BeanName 为键，以 Bean 实例为值，然后返回当前 Bean 实例。

```java
// 通过 ConcurrentHashMap（线程安全） 实现单例注册表
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<String, Object>(64);

public Object getSingleton(String beanName, ObjectFactory<?> singletonFactory) {
        Assert.notNull(beanName, "'beanName' must not be null");
        synchronized (this.singletonObjects) {
            // 检查缓存中是否存在实例
            Object singletonObject = this.singletonObjects.get(beanName);
            if (singletonObject == null) {
                //...省略了很多代码
                try {
                    singletonObject = singletonFactory.getObject();
                }
                //...省略了很多代码
                // 如果实例对象在不存在，我们注册到单例注册表中。
                addSingleton(beanName, singletonObject);
            }
            return (singletonObject != NULL_OBJECT ? singletonObject : null);
        }
    }
    //将对象添加到单例注册表
    protected void addSingleton(String beanName, Object singletonObject) {
            synchronized (this.singletonObjects) {
                this.singletonObjects.put(beanName, (singletonObject != null ? singletonObject : NULL_OBJECT));

            }
        }
}
```



当 Bean 的作用域设置成 Singleton 单例模式时存在线程安全问题。

**解决方法**

1）在 Bean 中尽量避免定义可变的成员变量

2）在类中定义一个 ThreadLocal 成员变量，将需要的可变成员变量保存在 ThreadLocal 中。

大部分情况下 Bean 都是无状态（没有实例变量） 比如 DAO、Service ，这种情况下 Bean 是线程安全的。



### 15.Spring 代理模式 AOP 和 TranSactional 注解

AOP 基于动态代理

**实现方式**

1）CGLIB 创建代理对象的子类 SpringBoot2 中默认用 CGLIB

2）JDK 动态代理，利用 Proxy 类的newProxyInstance 方法放入类加载器，要增强的接口数组，一个实现了 InnvocationHandler 接口的类，或者直接使用匿名内部类。

JDK 动态代理使用于有实现接口的对象。

Transactional 事务注解实现原理的 AOP 代理对象进行增强，因此 AOP 不能进行同类方法自调用，会导致 Spring 事务的失效

### 16.Spring 的模板方法模式

 Spring 中 JdbcTemplate、HibernateTemplate 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。一般情况下，我们都是使用继承的方式来实现模板模式，但是 Spring 并没有使用这种方式，而是使用 Callback 模式与模板方法模式配合，既达到了代码复用的效果，同时增加了灵活性。

### 17.Spring 的观察者模式

观察者模式是一种对象行为型模式，当一个对象发生改变时，所有依赖这个对象的对象也会做出改变。比如每次添加商品要更改商品索引。

**Spring 事件驱动模式三种角色**

**事件角色**

ApplicationEvent (org.springframework.context包下)充当事件的角色,这是一个抽象类，它继承了java.util.EventObject并实现了 java.io.Serializable接口。

- ContextStartedEvent：ApplicationContext 启动后触发的事件;
- ContextStoppedEvent：ApplicationContext 停止后触发的事件;
- ContextRefreshedEvent：ApplicationContext 初始化或刷新完成后触发的事件;
- ContextClosedEvent：ApplicationContext 关闭后触发的事件。

![img](https://xxx.xiaobaitiao.icu/2024/202403151504020.png)

**事件监听者**

ApplicationListener 充当了事件监听者角色，它是一个接口，里面只定义了一个 onApplicationEvent（）方法来处理ApplicationEvent。ApplicationListener接口类源码如下，可以看出接口定义看出接口中的事件只要实现了 ApplicationEvent就可以了。所以，在 Spring 中我们只要实现 ApplicationListener 接口的 onApplicationEvent() 方法即可完成监听事件

```java
package org.springframework.context;
import java.util.EventListener;
@FunctionalInterface
public interface ApplicationListener<E extends ApplicationEvent> extends EventListener {
    void onApplicationEvent(E var1);
}
```


**事件发布者角色**

```java
@FunctionalInterface
public interface ApplicationEventPublisher {
    default void publishEvent(ApplicationEvent event) {
        this.publishEvent((Object)event);
    }

    void publishEvent(Object var1);
}
```



**Spring 事件流程总结：**

1）定义一个事件：实现一个继承自 ApplicationEvent，并且写对应的构造函数。

2）定义一个事件监听者：实现 ApplicationListener 接口，重写 onApplicationEvent 方法。

3）使用事件发布者发布消息，通过 ApplicationEventPublisher 的 pulishEvent 方法发布消息。

例子

```java
// 定义一个事件,继承自ApplicationEvent并且写相应的构造函数
public class DemoEvent extends ApplicationEvent{
    private static final long serialVersionUID = 1L;

    private String message;

    public DemoEvent(Object source,String message){
        super(source);
        this.message = message;
    }

    public String getMessage() {
         return message;
          }


// 定义一个事件监听者,实现ApplicationListener接口，重写 onApplicationEvent() 方法；
@Component
public class DemoListener implements ApplicationListener<DemoEvent>{

    //使用onApplicationEvent接收消息
    @Override
    public void onApplicationEvent(DemoEvent event) {
        String msg = event.getMessage();
        System.out.println("接收到的信息是："+msg);
    }

}
// 发布事件，可以通过ApplicationEventPublisher  的 publishEvent() 方法发布消息。
@Component
public class DemoPublisher {

    @Autowired
    ApplicationContext applicationContext;

    public void publish(String message){
        //发布事件
        applicationContext.publishEvent(new DemoEvent(this, message));
    }
}
```

### 18.Spring 的适配器模式 AOP 和 Spring MVC 

适配器模式：可以将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的类可以一起工作。

**Spring AOP 的增强或通知使用到了适配器模式，接口是 AdvisorAdapter。**

Spirng 预定义的通知要通过对应的适配器，去适配成 MethodInterceptor 接口方法的对象，也就是说通过适配器 MethodBeforeAdviceAdapter 通过调用 getInterceptor 方法，将 MethodBeforeAdvice 适配成 MethodBeforeAdvice Interceptor。

**Spring MVC 使用适配器，将 Handler Mapping 做了一层适配，通过 Handler Adapter 去寻找对象的 Controller 层。**

为什么要使用？因为 SpringMVC 的 Controller 种类众多，不同类型的 Controller 通过不同方法对请求进行处理，如果不使用适配器直接获取对应类型的 Controller ，需要自行判断，会增加很多 if else ，而且当 Controller 烈性增加时，要在源代码上进行加入一行代码，**违反了设计模式的开闭原则，对扩展开放，对修改关闭。**



### 19.Spring 装饰器模式

在 JDK 中很多地方用到了装饰器模式，装饰器模式可以动态给对象添加一些额外的属性或者行为，相比于直接使用继承，更加灵活，像 InputStream 文件输入流，BufferedInputStream 缓冲区的增加就是利用了装饰器模式。

Spring 在配置 DataSource 数据源的时候，DateSource 可能是不同的数据库和数据源，那么利用装饰器模式，可以少修改原有类的代码下动态切换不同的数据源，一般类名包含 Wrapper 和 Decorator。