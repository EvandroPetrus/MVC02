1 - > run dotnet new mvc | dotnet new xunit | dotnet new console | dotnet new webapi
2 - > run dotnet add package Microsoft.EntityFrameworkCore.Design
3 - > run Microsoft.EntityFrameworkCore.SQLServer (if working with SQLServer)
4 - > create Entities folder(tabelas do DB)
5 - > create Context folder and XXXXContext as a class
6 - > XXXXContext will inherit DBContext (using Microsoft.EntityFrameworkCore)
7 - > create on XXXXContext :
    {
    public class AgendaContext : DbContext
    {
        public AgendaContext(DbContextOptions<AgendaContext> options) : base(options)
        {
            
        }

        public DbSet<Contato> Contatos{ get; set; }
    }
}
8 - > make connection on appsettings.Development for testing or on appsettings for real work ()
9 - > on program.cs, under //add services to container:

 builder.Services.AddDbContext<AgendaContext>(option =>
option.UseSqlServer(builder.Configuration.GetConnectionString("ConexaoPadrao")));     IF USING SQLSERVER !!

10 - > make sure SQLSERVER is running and then use dotnet -ef migrations new NOMEACAOTABELA (AdicionarTabelaContato)
11 - > dotnet-ef database update 
12 - > crete Controller for what you want to use
13 - > in the controller created use
     [ApiController]
     [Route("[controller])]
     public class COntatoController : ControllerBase
     {
        private readonly AgendaContext _context;
        {
            _context = context;
        }

        public COntatoController(AgendaContext Context)
        {
            _context = context;
        }
        
        [HttpPost]
        public IactionResult Create (Contato contato)
        {
            _context.Add(contato);
            _context.SaveChanges();

        }
    }

14 - > dotnet watch run -> swagger running correctly.

ONLY FOR API!!

For MVC do:
dotnet new mvc
and then use the above indications up to 13 //instead of entities use models

    