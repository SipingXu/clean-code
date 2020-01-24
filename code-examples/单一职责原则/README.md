**一个类或函数，只负责一件事，只有一个引起它变化的原因。**

---------

当函数做的事情不止一件事时，它们就很难编写，测试和阅读。如下代码所示：

```java
public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        Client clientRecord = repository.findOne(client.getId());
        if (clientRecord.isActive()){
            email(client);
        }
    }
}
```

重构后的代码：

```java
public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        if (isActiveClient(client)) {
            email(client);
        }
    }
}

private boolean isActiveClient(Client client) {
    Client clientRecord = repository.findOne(client.getId());
    return clientRecord.isActive();
}
```