**一段代码是否易于扩展，某段代码在应未来需求变化的时候，能够做到“对外开放，对修改关闭”，那就说明这段代码的扩展性比较好。**

```java
public class MessageValidate {
    public boolean validate(String userName, String phone){
        if(StringUtils.isEmpty(userName)){
            return false;
        }
        if(phone.length() == 1){
            return false;
        }
        return true;
    }
}
```

* 如果我们需要新增一个报文校验，就会对这个接口的代码做修改，对应的单元测试全部需要改；
* 这块属于校验的核心功能，每次改还要把之前的功能测试一遍。

重构后的代码如下(责任链模式)：

```java
@Data
public class OcpMessage
{
    private String id;
    private String userName;
    private String pwd;
    private String phone;
    private BigDecimal balance;
}

public abstract class AbastractValidator {
    public abstract boolean validate(OcpMessage message);
}

public class PhoneValidator extends AbastractValidator{
    @Override
    public boolean validate(OcpMessage ocpMessage) {
        if(ocpMessage.getPhone().length() == 1){
            return false;
        }
        return true;
    }
}

public class UserNameValidator extends AbastractValidator{
    @Override
    public boolean validate(OcpMessage ocpMessage) {
        if(StringUtils.isEmpty(ocpMessage.getUserName())){
            return false;
        }
        return true;
    }
}

public class Validators {
    private List<AbastractValidator> validators = new ArrayList<>();
    public void addValidator(AbastractValidator validator) {
        this.validators.add(validator); 
    }

    public void validate(OcpMessage ocpMessage) {
        for (AbastractValidator validator : validators) {
            validator.validate(ocpMessage);
        }
    }
}
```

