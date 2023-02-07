# ncc_test
https://tung-td.github.io/ncc_test/

-------------------------------- Master.cs

using System.Data;
using System.Data.SqlClient;

namespace LAST
{
    public partial class DanhMuc : System.Web.UI.MasterPage
    {
        string stnc = @"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=D:\ASP_E\LAST\LAST\App_Data\Database1.mdf;Integrated Security=True";
        protected void Page_Load(object sender, EventArgs e)
        {
            if (Page.IsPostBack) return;
            try
            {
                string q = "select * from LOAISACH";
                SqlDataAdapter da = new SqlDataAdapter(q, stnc);
                DataTable dt = new DataTable();
                da.Fill(dt);
                this.DataList1.DataSource = dt;
                this.DataList1.DataBind();
            }
            catch (SqlException ex)
            {
                Response.Write(ex.Message);
            }
        }

        protected void LinkButton1_Click(object sender, EventArgs e)
        {
            string maloai = ((LinkButton)sender).CommandArgument;
            Context.Items["ml"] = maloai;
            Server.Transfer("SanPham.aspx");
        }
    }
}

-------------------------------- Master

<asp:DataList ID="DataList1" runat="server">
    <ItemTemplate>
        <asp:LinkButton ID="LinkButton1" runat="server" 
            OnClick="LinkButton1_Click" 
            Text='<%# Eval("tenloai") %>' 
            CommandArgument='<%# Eval("maloai") %>'>
        </asp:LinkButton>
    </ItemTemplate>
</asp:DataList>

-------------------------------- Sanpham.cs

using System.Data;
using System.Data.SqlClient;

namespace LAST
{
    public partial class SanPham : System.Web.UI.Page
    {
        string stnc = @"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=D:\ASP_E\LAST\LAST\App_Data\Database1.mdf;Integrated Security=True";
        protected void Page_Load(object sender, EventArgs e)
        {
            if (Page.IsPostBack) return;

            string q;
            if (Context.Items["ml"] == null)
                q = "Select * from SACH";
            else
            {
                string maloai = Context.Items["ml"].ToString();
                q = "Select * from SACH where maloai = '" + maloai + "'";
            }
            try
            {
                SqlDataAdapter da = new SqlDataAdapter(q, stnc);
                DataTable dt = new DataTable();
                da.Fill(dt);
                this.DataList1.DataSource = dt;
                this.DataList1.DataBind();
            }
            catch(SqlException ex)
            {
                Response.Write(ex.Message);
            }
        }
    }
}


-------------------------------- Sanpham
<asp:Content ID="Content2" ContentPlaceHolderID="ContentPlaceHolder1" runat="server">

    <asp:DataList ID="DataList1" runat="server">
        <ItemTemplate>
            <asp:LinkButton ID="LinkButton1" runat="server" Text='<%# Eval("tensach") %>'></asp:LinkButton>

            <asp:Image ID="Image1" runat="server" ImageUrl='<%# "~/img/" + Eval("hinh") %>' Width="100px" Height="200px"/>

            <div class="money">
                Price: <%# Eval("dongia") %>
            </div>

        </ItemTemplate>
    </asp:DataList>
    
</asp:Content>
