# ABP Module 模块
1. ## Module和Assembly的关系
>ASP.NET Boilerplate provides an infrastructure to build modules and compose them to create an application. A module can depend on another module. Generally, an assembly is considered as a module. If you created an application with more than one assembly, it's suggested to create a module definition for each assembly.
Module system is currently focused on server side rather than client side.

>Module是ABP提供的一个架构（实际上是一种抽象化的功能），旨在提供应用程序的compose组合开发；模块之间可以配置依赖关系，一个Assembly可视为一个Module，推荐的做法是每一个Assembly创建一个Module。其实Assembly间有引用关系，为什么还要在Module间配置依赖关系呢？

    [DependsOn(typeof(MyBlogCoreModule))]
    public class MyBlogApplicationModule : AbpModule
    {
        public override void Initialize()
        {
            IocManager.RegisterAssemblyByConvention(Assembly.GetExecutingAssembly());
        }
    }

Module definition class is responsible to register it's classes to dependency injection (can be done conventionally as shown above) if needed. It can configure application and other modules, add new features to the application and so on...

定义一个Module类的职责是注册该类的DI(依赖注入，这里使用惯例注入)，这个定义里可以配置应用程序，也可配置其它Module,还可添加新功能

A module can be dependent to another. It's required to explicitly declare dependencies using DependsOn attribute。
Thus, we declare to ASP.NET Boilerplate that MyBlogApplicationModule depends on MyBlogCoreModule and the MyBlogCoreModule should be initialized before the MyBlogApplicationModule.
ABP can resolve dependencies recursively beginning from the startup module and initialize them accordingly. Startup module initialized as the last module.

2. ## Module的生命周期

        /// <summary>
        /// This is the first event called on application startup. 
        /// Codes can be placed here to run before dependency injection registrations.
        /// </summary>
        public virtual void PreInitialize()
        {

        }

        可以在依赖DI前执行的

        /// <summary>
        /// This method is used to register dependencies for this module.
        /// </summary>
        public virtual void Initialize()
        {

        }

        执行本Module的DI

        /// <summary>
        /// This method is called lastly on application startup.
        /// </summary>
        public virtual void PostInitialize()
        {
            
        }

        /// <summary>
        /// This method is called when the application is being shutdown.
        /// </summary>
        public virtual void Shutdown()
        {
            
        }   

        在 AbpModuleManager 中
         public virtual void StartModules()
        {
            var sortedModules = _modules.GetSortedModuleListByDependency(); 根据依赖排序
            sortedModules.ForEach(module => module.Instance.PreInitialize());
            sortedModules.ForEach(module => module.Instance.Initialize());
            sortedModules.ForEach(module => module.Instance.PostInitialize());
        }


3. ## Module的配置