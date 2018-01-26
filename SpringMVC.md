### Controller类

```java
@Controller
@RequestMapping(value = "请求前缀")
public class 类名 {
    @RequestMapping(value = "请求路径")
  	@RequestMapping(value = {"请求路径1","请求路径2"})
  	@RequestMapping(value = "请求路径", method=RequestMethod.请求方式)
    public String list(Model model, @RequestParam(value="请求参数")){
        return "视图地址";
      	return "redirect:视图地址";
      	return "forward:视图地址";
    }
```

