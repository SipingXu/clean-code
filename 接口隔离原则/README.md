**设计职责更加单一的接口，单一就意味着通用、复用性好；不要强制实现类重写一些不需要的方法。**

```java
public interface CommonService {
    public List<String> queryUserName(String status);
    public int deleteUser(Long id);
    public List<String> queryRoleName(String userName);
    public int deleteRole(Long id);
}

public class RoleServiceImpl implements CommonService{
    @Override
    public List<String> queryUserName(String status) {
        return null;
    }

    @Override
    public int deleteUser(Long id) {
        return 0;
    }

    @Override
    public List<String> queryRoleName(String userName) {
        return null;
    }

    @Override
    public int deleteRole(Long id) {
        return 0;
    }
}

public class UserServiceImpl implements CommonService{
    @Override
    public List<String> queryUserName(String status) {
        return null;
    }

    @Override
    public int deleteUser(Long id) {
        return 0;
    }

    @Override
    public List<String> queryRoleName(String userName) {
        return null;
    }
    
    @Override
    public int deleteRole(Long id) {
        return 0;
    }
}
```

* 接口不灵活，用户和角色的接口放在一起要多做一个工作量
* 复用性很差
* 不利于代码拆分、管理

重构后的代码如下：

```java
public interface UserService {
    public List<String> queryUserName(String status);
    public int deleteUser(Long id);
}

public interface RoleService {
    public List<String> queryRoleName(String userName);
    public int deleteRole(Long id);
}

public class RoleServiceImpl implements RoleService {
    @Override
    public List<String> queryRoleName(String userName) {
        return null;
    }

    @Override
    public int deleteRole(Long id) {
        return 0;
    }
}

public class UserServiceImpl implements UserService{
    @Override
    public List<String> queryUserName(String status) {
        return null;
    }

    @Override
    public int deleteUser(Long id) {
        return 0;
    }
}
```

