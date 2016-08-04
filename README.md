//program.cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.SqlClient;
using System.Data;
namespace assesment1
{

   public  class Program
    {
        
        static void Main(string[] args)
        {
            empdb obj = new empdb();
            int choice;
            do
            {
                Console.WriteLine("enter your choice");
                choice = Convert.ToInt32(Console.ReadLine());
                switch (choice)
                {
                    case 1:
                        
                        Console.WriteLine("enter name");
                        int id = Convert.ToInt32(Console.ReadLine());
                        string name = Console.ReadLine();
                        string location = Console.ReadLine();
                        string band = Console.ReadLine();
                        emp1 e = new emp1(id, name, location, band);
                        int result = obj.addemp(e);
                        Console.WriteLine("customer added " + result);
                        break;
                    case 2:
                        Console.WriteLine("enter id");
                        int id1 = Convert.ToInt32(Console.ReadLine());
                        List<emp1> listt = obj.viewemp();
                       
                        foreach(emp1 e1 in listt)
                        {
                            Console.WriteLine(e1.ID);
                            Console.WriteLine(e1.Name);
                            Console.WriteLine(e1.Location);
                            Console.WriteLine(e1.Band);
                        }
                        break;


                      


                }




            } while (choice != 5);
            Console.ReadLine();

         }



        
    }
}
//emp1.cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace assesment1
{
   public class emp1
    {
     
      public int id;
       public  string name;
       public  string location;
       public  string band;
        public int ID
        {
            get { return id; }
            set { id = value; }
        }
        public string Name
        {
            get { return name; }
            set { name = value; }
        }
        public string Location
        {
            get { return location; }
            set { name = value; }
        }
        public string Band
        {
            get { return band; }
            set { name = value; }
 
        }
        public emp1(int id1, string name1, string location1, string band1)
        {
            id = id1;
            name = name1;
            location = location1;
            band = band1;
        }
        public emp1(string name1, string location1, string band1)
        {

            name = name1;
            location = location1;
            band = band1;

        }
    }
}
//empdb.cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.SqlClient;
using System.Data;
namespace assesment1
{
   public class empdb
    {
       public List<emp1> viewemp()
       {
           string connectionstring = @"Data Source=RISHU\SQLSERVER2;Initial Catalog=ass1;User ID=sa;Password=pass";
           SqlConnection con = new SqlConnection(connectionstring);
           con.Open();
           SqlCommand cmd = new SqlCommand();
           cmd.CommandType = CommandType.StoredProcedure;
           cmd.CommandText = "sp_view";
           cmd.Connection = con;
           SqlDataReader dr = cmd.ExecuteReader();
           cmd.Parameters.AddWithValue("@id",);
           List<emp1> listt = new List<emp1>();
          
           while (dr.Read())
           {
               int id = Convert.ToInt32(dr["id"]);
               string name = Console.ReadLine();
               string location = Console.ReadLine();
               string band = Console.ReadLine();
               emp1 obj1 = new emp1(id, name, location, band);
               listt.Add(obj1);
           }
           con.Close();
           return listt;
       }
        public int addemp(emp1 obj1)
        {
 string connectionstring=@"Data Source=RISHU\SQLSERVER2;Initial Catalog=ass1;User ID=sa;Password=pass";
 SqlConnection con = new SqlConnection(connectionstring);
 con.Open();
 SqlCommand cmd = new SqlCommand();
 cmd.CommandType = CommandType.StoredProcedure;
 cmd.CommandText = "sp_insertt";
 cmd.Connection = con;
 cmd.Parameters.AddWithValue("@id", obj1.ID);
 cmd.Parameters.AddWithValue("@name", obj1.Name);
 cmd.Parameters.AddWithValue("@location", obj1.Location);
 cmd.Parameters.AddWithValue("@band", obj1.Band);
 int rowsaff = cmd.ExecuteNonQuery();
 con.Close();
 if (rowsaff > 0)
 {
     return Convert.ToInt32(cmd.Parameters["@id"].Value);
 }
 else
     return rowsaff;
        }
    }
}

//daTABASE
create procedure sp_insertt
(@id int,@name varchar(30),@location varchar(30),@band varchar(30))
as 
begin 
insert into emp values(@id,@name,@location,@band)
end
drop procedure sp_insertt
select * from insertt
create procedure sp_view
(@id int)
as
begin
select * from emp where id=@id
end
