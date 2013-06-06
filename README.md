### SpringMVC-helloworld

This is a very basic example of using Spring MVC with JavaConfig to make a helloworld web apps.

The first part of this is to create a configuration class for the web app.  below is a sample of the configuration class we are going to use:

    Configuration
    @EnableWebMvc
    @ComponentScan(basePackages = {"com.johnathanmsmith.mvc.web"})
    public class WebMVCConfig extends WebMvcConfigurerAdapter
    {

        private static final Logger logger = LoggerFactory.getLogger(WebMVCConfig.class);

        @Bean
        public ViewResolver resolver()
        {
            UrlBasedViewResolver url = new UrlBasedViewResolver();
            url.setPrefix("/views/");
            url.setViewClass(JstlView.class);
            url.setSuffix(".jsp");
            return url;
        }

        @Override
        public void addResourceHandlers(ResourceHandlerRegistry registry)
        {
            logger.debug("setting up resource handlers");
            registry.addResourceHandler("/resources/").addResourceLocations("/resources/**");
        }

        @Override
        public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer)
        {
            logger.debug("configureDefaultServletHandling");
            configurer.enable();
        }

        @Bean
        public SimpleMappingExceptionResolver simpleMappingExceptionResolver()
        {
            SimpleMappingExceptionResolver b = new SimpleMappingExceptionResolver();

            Properties mappings = new Properties();
            mappings.put("org.springframework.web.servlet.PageNotFound", "p404");
            mappings.put("org.springframework.dao.DataAccessException", "dataAccessFailure");
            mappings.put("org.springframework.transaction.TransactionException", "dataAccessFailure");
            b.setExceptionMappings(mappings);
            return b;
        }
    }


Next you have to setup the web.xml file to use the above configuration class, we do this but setting the contectConfigLocation to the package of the configuration class. see below:

    <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns="http://java.sun.com/xml/ns/javaee"
             xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
             version="2.5">

        <servlet>
            <servlet-name>Spring MVC Dispatcher Servlet</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            <init-param>
                <param-name>contextClass</param-name>
                <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
            </init-param>
            <init-param>
                <param-name>contextConfigLocation</param-name>
                <param-value>com.johnathanmsmith.mvc.web.config, com.johnathanmsmith.mvc.web.controller</param-value>
            </init-param>
            <load-on-startup>1</load-on-startup>
        </servlet>

        <servlet-mapping>
            <servlet-name>Spring MVC Dispatcher Servlet</servlet-name>
            <url-pattern>/</url-pattern>
        </servlet-mapping>
    </web-app>

Now lets setup a basic controller to display a page:

    @Controller
    @RequestMapping("/ask")
    class IndexController
    {

        private static final Logger logger = LoggerFactory.getLogger(IndexController.class);

        @RequestMapping(method = RequestMethod.GET)
        public String displayRequestPage()
        {
            /*
               I am going to display the helloworld.jsp page now :)
             */
            logger.debug("made it to controller");
            return "helloworld";

        }


    }

Thats all it takes..

## Getting The Project and Running It

To get this project and run it you will need to follow the following steps:

    git clone  git@github.com:JohnathanMarkSmith/springmvc-helloworld.git
    cd springmvc-helloworld/
    mvn tomcat7:run

Now open your web brower and goto http://127.0.0.1:8080/springmvc-helloworld/

This its... Have run with it...


If you have any questions or comments please email me at john@johnathanmarksmith.com or checkout my web site http://JohnathanMarkSmith.com

