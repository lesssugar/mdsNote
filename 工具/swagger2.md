#  倒入约束

```
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.0</version>
</dependency>
```

# 写配置类

```java
@EnableSwagger2

/**
     * group :rest
     * 扫描注解了@ApiOperation的方法生成API接口文档
     * @return
     */	
    @Bean
    public Docket RestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("rest")
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .paths(PathSelectors.any())
                .build();
    }

    /**
     * group :app
     * 扫描controller包生成API接口文档
     * @return
     */
    @Bean
    public Docket packageApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("app")
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("tech.yiyehu.modules.app.controller"))  
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("帮帮校园")
                .description("帮帮校园")
                .termsOfServiceUrl("http://www.yiyehu.tech/")
                .version("1.0")
                .build();
    }
```

## 配置视图解析器

```java
@Configuration
public class WebMvcConfig extends WebMvcConfigurerAdapter {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("swagger-ui.html")
                .addResourceLocations("classpath:/META-INF/resources/");
    }

}
```

# 登录

```xml
http://localhost:9090/swagger/swagger-ui.html	//根据自己的配置即可访问页面
```

## 常用注解

```java
@Api：用在请求的类上，表示对类的说明  
    tags="说明该类的作用，可以在UI界面上看到的注解"  
    value="该参数没什么意义，在UI界面上也看不到，所以不需要配置"  
  
@ApiOperation：用在请求的方法上，说明方法的用途、作用  
    value="说明方法的用途、作用"  
    notes="方法的备注说明"
例子：
@ApiOperation(value="用户注册",notes="手机号、密码都是必输项，年龄随边填，但必须是数字") 
    
@ApiImplicitParams：用在请求的方法上，表示一组参数说明  
    @ApiImplicitParam：用在@ApiImplicitParams注解中，指定一个请求参数的各个方面  
        name：参数名  
        value：参数的汉字说明、解释  
        required：参数是否必须传  
        paramType：参数放在哪个地方  
            · header --> 请求参数的获取：@RequestHeader  
            · query --> 请求参数的获取：@RequestParam  
            · path（用于restful接口）--> 请求参数的获取：@PathVariable  
            · body（不常用）  
            · form（不常用）      
        dataType：参数类型，默认String，其它值dataType="Integer"         
        defaultValue：参数的默认值  
例子：
@ApiImplicitParams({  
    @ApiImplicitParam(name="mobile",value="手机号",required=true,paramType="form"),  
    @ApiImplicitParam(name="password",value="密码",required=true,paramType="form"),  
    @ApiImplicitParam(name="age",value="年龄",required=true,paramType="form",dataType="Integer")  
})  
    
@ApiResponses：用在请求的方法上，表示一组响应  
    @ApiResponse：用在@ApiResponses中，一般用于表达一个错误的响应信息  
        code：数字，例如400  
        message：信息，例如"请求参数没填好"  
        response：抛出异常的类  
例子：
@ApiResponses({  
    @ApiResponse(code=400,message="请求参数没填好"),  
    @ApiResponse(code=404,message="请求路径没有或页面跳转路径不对")  
})  
    
@ApiModel：用于响应类上，表示一个返回响应数据的信息  
            （这种一般用在post创建的时候，使用@RequestBody这样的场景，  
            请求参数无法使用@ApiImplicitParam注解进行描述的时候）  
    @ApiModelProperty：用在属性上，描述响应类的属性  
例子：
@ApiModel(description= "返回响应数据")  
public class RestMessage implements Serializable{  
  
    @ApiModelProperty(value = "是否成功")  
    private boolean success=true;  
    @ApiModelProperty(value = "返回对象")  
    private Object data;  
    @ApiModelProperty(value = "错误编号")  
    private Integer errCode;  
    @ApiModelProperty(value = "错误信息")  
    private String message;  
  
    /* getter/setter */  
}  
```

# 项目中实例

```
package com.thunisoft.znbapt.api.controller.rest;

import com.thunisoft.znbapt.cases.service.CaseService;
import com.thunisoft.znbapt.datamodel.api.*;
import com.thunisoft.znbapt.utils.RequestUtil;
import io.swagger.annotations.*;
import org.apache.http.HttpStatus;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import javax.servlet.http.HttpServletRequest;
import java.util.Map;

@Api(value = "案件接口", tags = "Case")
@RequestMapping("/api")
@RestController
public class CasesController {

    @Autowired
    private CaseService caseService;

    @RequestMapping(value = "/users/{userId}/cases", method = RequestMethod.GET)
    @ApiOperation(
            value = "查询案件信息",
            responseReference = "Query<Case>")
    @ApiImplicitParams(
            value = {
                    @ApiImplicitParam(
                            name = "caseStatus",
                            value = "案件状态",
                            required = false,
                            paramType = "String",
                            examples = @Example({
                                    @ExampleProperty(value = "waitConfirm", mediaType = "待确认"),
                                    @ExampleProperty(value = "processing", mediaType = "办理中"),
                                    @ExampleProperty(value = "closed", mediaType = "已结案")
                            })
                    ),
                    @ApiImplicitParam(
                            name = "keyword",
                            value = "查询关键字",
                            required = false,
                            paramType = "String",
                            examples = @Example({
                                    @ExampleProperty(value = "韩梅梅诉李雷等离婚纠纷一案", mediaType = "案件名称"),
                                    @ExampleProperty(value = "(2015)湘高院民初字第(00212)号", mediaType = "案号"),
                                    @ExampleProperty(value = "邓志刚", mediaType = "承办人"),
                                    @ExampleProperty(value = "肖芳", mediaType = "参与人")
                            })
                    ),
                    @ApiImplicitParam(
                            name = "ownerType",
                            value = "案件与人员所属关系",
                            required = false,
                            paramType = "String",
                            examples = @Example({
                                    @ExampleProperty(value = "myUndertake", mediaType = "我承办"),
                                    @ExampleProperty(value = "myInvolvement", mediaType = "我参与"),
                                    @ExampleProperty(value = "myCourt", mediaType = "本庭")
                            })
                    ),
                    @ApiImplicitParam(
                            name = "endStatus",
                            value = "结案状态",
                            required = false,
                            paramType = "String",
                            examples = @Example({
                                    @ExampleProperty(value = "closed", mediaType = "已结案"),
                                    @ExampleProperty(value = "archived", mediaType = "已归档")
                            })
                    ),
                    @ApiImplicitParam(
                            name = "closeTime",
                            value = "查询结案时间（之内）",
                            required = false,
                            paramType = "String"),
                    @ApiImplicitParam(
                            name = "beyondCloseTime",
                            value = "查询结案时间（之外）",
                            required = false,
                            paramType = "String"),
                    @ApiImplicitParam(
                            name = "hasCourt",
                            value = "查询是否开庭",
                            required = false,
                            paramType = "Boolean"),
                    @ApiImplicitParam(
                            name = "hasTrial",
                            value = "查询是否超审限",
                            required = false,
                            paramType = "Boolean"),
                    @ApiImplicitParam(
                            name = "hasDocumented",
                            value = "查询文书是否已完成",
                            required = false,
                            paramType = "Boolean"),
                    @ApiImplicitParam(
                            name = "pageNo",
                            value = "分页-页数",
                            required = false,
                            paramType = "Integer"),
                    @ApiImplicitParam(
                            name = "pageSize",
                            value = "分页-每页个数",
                            required = false,
                            paramType = "Integer"),
                    @ApiImplicitParam(
                            name = "orderBy",
                            value = "排序字段",
                            required = false,
                            paramType = "String",
                            examples = @Example({
                                    @ExampleProperty(value = "mixTrialTime", mediaType = "审限")
                            })
                    ),
                    @ApiImplicitParam(
                            name = "order",
                            value = "排序规则",
                            required = false,
                            paramType = "String",
                            examples = @Example({
                                    @ExampleProperty(value = "desc", mediaType = "倒序"),
                                    @ExampleProperty(value = "asc", mediaType = "正序")
                            })
                    )
            }
    )
    @ApiResponses(
            value = {
                    @ApiResponse(
                            code = HttpStatus.SC_NOT_FOUND,
                            message = "未查询到则返回404"
                    ),
                    @ApiResponse(
                            code = HttpStatus.SC_UNAUTHORIZED,
                            message = "返回登录地址",
                            response = Unauthorized.class
                    ),
                    @ApiResponse(
                            code = HttpStatus.SC_FORBIDDEN,
                            message = "返回注销地址",
                            response = Forbidden.class
                    )
            }
    )
    public Query<Case> getCases(@PathVariable String userId, HttpServletRequest request) {
        Map<String, String> params = RequestUtil.getParams(request);
        return caseService.getCase(userId, params);
    }

    @RequestMapping(value = "/cases/{caseId}/processes", method = RequestMethod.GET)
    @ApiOperation(
            value = "查询案件流程信息",
            response = Processes.class)
    @ApiImplicitParams(
            value = {
                    @ApiImplicitParam(
                            name = "ajlb",
                            value = "案件类别",
                            required = true,
                            paramType = "Integer")
            }
    )
    @ApiResponses(
            value = {
                    @ApiResponse(
                            code = HttpStatus.SC_NOT_FOUND,
                            message = "未查询到则返回404"
                    ),
                    @ApiResponse(
                            code = HttpStatus.SC_UNAUTHORIZED,
                            message = "返回登录地址",
                            response = Unauthorized.class
                    ),
                    @ApiResponse(
                            code = HttpStatus.SC_FORBIDDEN,
                            message = "返回注销地址",
                            response = Forbidden.class
                    )
            }
    )
    public Processes getProcesses(@PathVariable String caseId, HttpServletRequest request) {
        Map<String, String> params = RequestUtil.getParams(request);
        return caseService.getProcesses(caseId, params);
    }

    @RequestMapping(value = "/cases/sign", method = RequestMethod.GET)
    @ApiOperation(
            value = "签收案件,有承办人则为分案",
            response = Status.class)
    public Status confirmSign(
            @RequestParam(value = "cAjbhs") String cAjbhs,
            @RequestParam(value = "cCbr", required = false) String cCbr,
            @RequestParam(value = "dQsrq") String dQsrq,
            @RequestParam(value = "ajlb") String ajlb
    ) {
        return caseService.confirmSign(cAjbhs, cCbr, dQsrq, ajlb);
    }

    @RequestMapping(value = "/cases/public", method = RequestMethod.GET)
    @ApiOperation(
            value = "公开案件",
            response = Status.class)
    public Status ajgkByAjlb(
            @RequestParam(value = "cAjbhs") String cAjbhs,
            @RequestParam(value = "ajlb") String ajlb
    ) {
        return caseService.ajgkByAjlb(cAjbhs, ajlb);
    }
}
```

# 更换ui页面

```bash
https://gitee.com/xiaoym/swagger-bootstrap-ui
//失败的时候是因为可能Ui的jar包maven下载成功了但是也没有导入
```

