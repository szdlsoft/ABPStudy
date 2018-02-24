# ABP Module 模块
1. ## Module和Assembly的关系
ASP.NET Boilerplate provides an infrastructure to build modules and compose them to create an application. A module can depend on another module. Generally, an assembly is considered as a module. If you created an application with more than one assembly, it's suggested to create a module definition for each assembly.
Module system is currently focused on server side rather than client side.
  Module是ABP提供的一个架构（实际上是一种抽象化的功能），旨在提供应用程序的compose组合开发；模块之间可以配置依赖关系，一个Assembly可视为一个Module，推荐的做法是每一个Assembly创建一个Module。其实Assembly间有引用关系，为什么还要在Module间配置依赖关系呢？

    public class MyBlogApplicationModule : AbpModule
    {
        public override void Initialize()
        {
            IocManager.RegisterAssemblyByConvention(Assembly.GetExecutingAssembly());
        }
    }
    
2. ## Module的生命周期
3. ## Module的配置