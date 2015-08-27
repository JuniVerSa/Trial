# Singelton Pattern
  *   Този шаблон за дизайн се използва в обектно-ориентираното програмиране, обикновено при моделирането на обекти, които трябва да бъдат глобално достъпни за обектите на приложението или обекти, които се нуждаят от максимално късна инициализация за пестене на ресурси от паметта. Характерно за класовете, които имплементират Singelton шаблона, е че могат да имат една единствена инстанция, например достъп до файловат система, конзолата, глобален логер, мапър и др.
  *   Singelton шаблонът се изпълнява, като се прави клас с метод, който прави нова инстанция на класа, ако не съществува. Ако инстанцията съществува, просто връща референцията на този обект. За да е сигурно, че обектът не може да бъде инстанциран по друг начин, конструкторът се прави частен. Singelton шаблонът трябва да бъде внимателно конструиран в многонишковите апликации. Ако две нишки стигнат до създаването на инстанция по едно и също време, е възможно да се създадат две инстанции на класа, което нарушава замисъла на Singelton шаблона. 
  *   Възможни имплементации
      *   При еднонишково програмиране (__not thread-safe__):
        
            // public sealed class Singleton
            // {
               // private static Singleton instance=null;

               // private Singleton()
               // {
               // }

               // public static Singleton Instance
               // {
                     // get
                     // {
                           // if (instance==null)
                           // {
                                 // instance = new Singleton();
                           // }
                           // return instance;
                     // }
               // }
            // }
      * __simple thread-safety__:
           
             // public sealed class Singleton
             // {
                   // private static Singleton instance = null;
                   // private static readonly object padlock = new object();

                   // Singleton()
                   // {
                   // }

                   // public static Singleton Instance
                   // {
                          // get
                          // {
                                // lock (padlock)
                                // {
                                       // if (instance == null)
                                       // {
                                              // instance = new Singleton();
                                       // }
                                       // return instance;
                                // }
                          // }
                   // }
             // }
     *__thread-safety using double-check locking__
           // public sealed class Singleton
           // {
           // private static Singleton instance = null;
           // private static readonly object padlock = new object();

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
