---
id: spring-boot-validation
title: 参数校验
sidebar_label: 参数校验
---

## 依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

## 常用注解

| 注解                                           | 运行时检查                                                   |
| ---------------------------------------------- | ------------------------------------------------------------ |
| @AssertFalse                                   | 被注解的元素必须为false                                      |
| @AssertTrue                                    | 被注解的元素必须为true                                       |
| @DecimalMax(value)                             | 被注解的元素必须为一个数字，其值必须小于等于指定的最小值     |
| @DecimalMin(Value)                             | 被注解的元素必须为一个数字，其值必须大于等于指定的最小值     |
| @Digits(integer=, fraction=)                   | 被注解的元素必须为一个数字，其值必须在可接受的范围内         |
| @Future                                        | 被注解的元素必须是日期，检查给定的日期是否比现在晚           |
| @Max(value)                                    | 被注解的元素必须为一个数字，其值必须小于等于指定的最小值     |
| @Min(value)                                    | 被注解的元素必须为一个数字，其值必须大于等于指定的最小值     |
| @NotNull                                       | 被注解的元素必须不为null                                     |
| @Null                                          | 被注解的元素必须为null                                       |
| @Past(java.util.Date/Calendar)                 | 被注解的元素必须过去的日期，检查标注对象中的值表示的日期比当前早 |
| @Pattern(regex=, flag=)                        | 被注解的元素必须符合正则表达式，检查该字符串是否能够在match指定的情况下被regex定义的正则表达式匹配 |
| @Size(min=, max=)                              | 被注解的元素必须在制定的范围(数据类型:String, Collection, Map and arrays) |
| @Valid                                         | 递归的对关联对象进行校验, 如果关联对象是个集合或者数组, 那么对其中的元素进行递归校验,如果是一个map,则对其中的值部分进行校验 |
| @CreditCardNumber                              | 对信用卡号进行一个大致的验证                                 |
| @Email                                         | 被注释的元素必须是电子邮箱地址                               |
| @Length(min=, max=)                            | 被注解的对象必须是字符串的大小必须在制定的范围内             |
| @NotBlank                                      | 被注解的对象必须为字符串，不能为空，检查时会将空格忽略       |
| @NotEmpty                                      | 被注释的对象必须为空(数据:String,Collection,Map,arrays)      |
| @Range(min=, max=)                             | 被注释的元素必须在合适的范围内 (数据：BigDecimal, BigInteger, String, byte, short, int, long and 原始类型的包装类 ) |
| @URL(protocol=, host=, port=, regexp=, flags=) | 被注解的对象必须是字符串，检查是否是一个有效的URL，如果提供了protocol，host等，则该URL还需满足提供的条件 |

## Request

```java
@Data
public class XxxRequest {

    @NotEmpty(message = "姓名不能为空")
    private String name;

    @Min(value = 1 , message = "年龄不能小于1岁")
    private Integer age;

    @NotEmpty(message = "性别不能为空")
    private Integer sex;

}
```

## ExceptionHandler

```java
@RestControllerAdvice
@Slf4j
public class ExceptionHandler {

    /**
     * 请求参数异常处理
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity handleMethodArgumentNotValidException(MethodArgumentNotValidException ex) {
        Map<String, Object> errors = new HashMap<>(8);
        ex.getBindingResult().getAllErrors().forEach(error -> {
            String fieldName = ((FieldError) error).getField();
            String errorMessage = error.getDefaultMessage();
            errors.put(fieldName, errorMessage);
        });
        log.error("occur MethodArgumentNotValidException:" + errors);
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errors);
    }
}
```

## [参考](https://gitee.com/huangxunhui/unifiedParamCheck)
