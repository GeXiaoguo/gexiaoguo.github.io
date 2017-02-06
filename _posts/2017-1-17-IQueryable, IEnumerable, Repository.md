# IQueryable, IEnumerable and Repositories #

When implementing the Repository pattern against a database storage, one always has to choose from returning an IEnumerable or an IQueryable collection. I'd say neither is a good choice. Instead, the following pattern should be applied for the following reasons
  

    public List<Entity> GetEntity()  
    {  
      IEnumerable<Entities> entities;  
      using (var context = new DBContext())  
      {    
        entities = context.Articles.ToList();  
      }  
      return entities;  
    }  


1. [DataContext should be short lived](https://blogs.msdn.microsoft.com/dinesh.kulkarni/2008/04/28/lifetime-of-a-linq-to-sql-datacontext/ "DataContext should be short lived")  

    DataContext are designed to be short lived, [lightweight](https://msdn.microsoft.com/en-us/library/system.data.linq.datacontext.aspx#0cb87d0d-0981-48f1-ab1a-6fc0fdfa5e9f_c "MSDN") and stateless. We should take advantage of this feature. By not maintaining a long lived DataContext makes our own code clean and easy to understand and maintain.
2. IQueryable and IEnumerable should not out live the DataContext   
    Both IQueryable and IEnumerable are lazy evaluated. If they out live the DataContext, any attempt to retrieve data would cause an exception.


3. IQueryable has the Accidental Lazy Loading problem
    Besides the lazy evaluation problem mentioned above, IQueryable has its own unique Accidental Lazy Loading problem. It allows database queries to be made outside the data access layer, which its sole purpose to have is to capture all the database accesses. And the database queries are made in such a implicity way that nobody will notice until they start to cause problem in production

    And having a short lived DataContext would raise an exception in this case. It exposes the problem in development rather in production.
    
    For example: suppose a collection of book entities is returned as a IQueryable type,


        @foreach(var book in Model.Books) {
        <div>
            <b>@book.Title</b>
        </div>

    If a new requirement comes in to display additionally the total review count of a book, this would require a simple code change on the UI.

        @foreach(var book in Model.Books) {
        <div>
            <b>@book.Title</b>
            <span>@book.Comments.Count() comments</span>
        </div>


    It compiles and runs fine. But behind everybody's back, and for every book, it would hit the database asking for the Comments and count. The problem would only show up underload and that is the worst time for it to show up



references:  
[http://stackoverflow.com/questions/10777630/questions-about-entity-framework-context-lifetime](http://stackoverflow.com/questions/10777630/questions-about-entity-framework-context-lifetime)
[http://stackoverflow.com/questions/10777630/questions-about-entity-framework-context-lifetime](http://stackoverflow.com/questions/10777630/questions-about-entity-framework-context-lifetime)
