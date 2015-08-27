# Singelton Pattern
  *   Този шаблон за дизайн се използва в обектно-ориентираното програмиране, обикновено при моделирането на обекти, които трябва да бъдат глобално достъпни за обектите на приложението или обекти, които се нуждаят от максимално късна инициализация за пестене на ресурси от паметта. Характерно за класовете, които имплементират Singelton шаблона, е че могат да имат една единствена инстанция, например достъп до файловат система, конзолата, глобален логер, мапър и др.
  *   Singelton шаблонът се изпълнява, като се прави клас с метод, който прави нова инстанция на класа, ако не съществува. Ако инстанцията съществува, просто връща референцията на този обект. За да е сигурно, че обектът не може да бъде инстанциран по друг начин, конструкторът се прави частен. Singelton шаблонът трябва да бъде внимателно конструиран в многонишковите апликации. Ако две нишки стигнат до създаването на инстанция по едно и също време, е възможно да се създадат две инстанции на класа, което нарушава замисъла на Singelton шаблона. 
  *   Възможни имплементации
      *   __not thread-safe__:
        
             public sealed class Singleton
             {
                private static Singleton instance=null;

                private Singleton()
                {
                }

                public static Singleton Instance
                {
                      get
                      {
                            if (instance==null)
                            {
                                  instance = new Singleton();
                            }
                            return instance;
                      }
                }
             }
      * __simple thread-safety__:
           
             public sealed class Singleton
             {
                   private static Singleton instance = null;
                   private static readonly object padlock = new object();

                   Singleton()
                   {
                   }

                   public static Singleton Instance
                   {
                          get
                          {
                                lock (padlock)
                                {
                                       if (instance == null)
                                       {
                                              instance = new Singleton();
                                       }
                                       return instance;
                                }
                          }
                   }
             }
      * __thread-safety using double-check locking__:
      
             public sealed class Singleton
             {
                   private static Singleton instance = null;
                   private static readonly object padlock = new object();

                   Singleton()
                   {
                   }

                   public static Singleton Instance
                   {
                          get
                          {
                                if (instance == null)
                                {
                                      lock (padlock)
                                      {
                                           if (instance == null)
                                           {
                                                  instance = new Singleton();
                                           }
                                      }
                                }
                                return instance;
                          }
                   }
             }
      *  __thread-safe without using locks__:
     
             public sealed class Singleton
             {
                   private static readonly Singleton instance = new Singleton();

                   // Explicit static constructor to tell C# compiler
                   // not to mark type as beforefieldinit
                   static Singleton()
                   {
                   }

                   private Singleton()
                   {
                   }

                   public static Singleton Instance
                   {
                         get
                         {
                               return instance;
                         }
                   }
             }
      *  __fully lazy instantiation__:
      
             public sealed class Singleton
             {
                   private Singleton()
                   {
                   }

                   public static Singleton Instance { get { return Nested.instance; } }
        
                   private class Nested
                   {
                          // Explicit static constructor to tell C# compiler
                          // not to mark type as beforefieldinit
                          static Nested()
                          {
                          }

                          internal static readonly Singleton instance = new Singleton();
                   }
             } 
      * __using .NET 4's Lazy<T> type__:
      
             public sealed class Singleton
             {
                 private static readonly Lazy<Singleton> lazy =
                 new Lazy<Singleton>(() => new Singleton());
    
                 public static Singleton Instance { get { return lazy.Value; } }

                 private Singleton()
                 {
                 }
             }
  *   Базовата имплементация (__not thread safe__) не трябва да се използва в мнгонишкови среди (напр.ASP.NET). Singleton нарушава един от __SOLID__ принципите (__SRP--), защото класът изпълнява повече от една задача едновременно и освен това води до силна връзка между свързаните класове (tight coupling). Singleton шаблоните са трудни за тестване.
  *   Моделите: __The Abstract Factory__, __Builder__ и __Prototype__ могат да използват Singleton при тяхното изпълнение. __Facade__ и __State__ шаблоните също могат да използват Singleton.
