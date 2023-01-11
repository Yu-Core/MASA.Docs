## 3. 自定义仓储实现

虽然框架已经提供了仓储功能, 但它的功能是有限的, 当默认仓储提供的功能不足以满足我们的需求时, 我们就需要在默认仓储的基础上进行扩展或者重写, 自定义仓储与实现是一对一的, 它们必须是成对出现的

### 使用

在项目根目录新建`Infrastructure`文件夹, 并在其中新建`Repositories`文件夹, 用于存放自定义仓储的实现

```csharp
public class CatalogItemRepository : Repository<CatalogDbContext, CatalogItem, Guid>, ICatalogItemRepository
{
    public CatalogItemRepository(CatalogDbContext context, IUnitOfWork unitOfWork) : base(context, unitOfWork)
    {
    }
}
```

自定义仓储实现可以继承`Repository<CatalogDbContext, CatalogItem, Guid>`, 我们只需要在默认仓储实现的基础上扩展新扩展的方法即可, 如果你不满意默认实现, 也可重写父类的方法, 默认仓储支持了很多功能, 查看详细[文档](/framework/building-blocks/ddd/repository)

无论是直接使用框架提供的仓储能力, 还是基于默认仓储提供的能力基础上进行扩展, 都需要我们在`Program.cs`中进行注册, 否则仓储将无法正常使用, 例如:

```csharp
builder.Services.AddDomainEventBus(options =>
{
    options.UseRepository<CatalogDbContext>();
});
```

框架是如何完成自动注册, 为何项目提示仓储未注册, 点击查看[文档](/framework/building-blocks/ddd/repository)

> 如果不在默认仓储的的基础上扩展, 而是完全自定义仓储, 则可以使用[`按约定自动注册`](/framework/utils/extensions/dependency-injection)功能简化服务注册