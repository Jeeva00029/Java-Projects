//foodorderapp.java class

package fdmanage;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
public class Foodordapp {
	//server details
	String url="jdbc:mysql://localhost:3306/food_order_mgement_sys";
	String user="root";
	String pass="accord";
	Connection con;
	public Foodordapp() throws SQLException {
		con=DriverManager.getConnection(url,user,pass);
	}
	//check login id and pass for regular user
	public boolean userlog(int userid,String pass) throws SQLException {
		boolean res=false;
		String q="select * from users where UserID=? and Password=?";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1, userid);
		pst.setString(2, pass);
		ResultSet rs=pst.executeQuery();
		while(rs.next()) {
			res=true;
		}
		return res;
	}
	//check login id and pass for admin user
	public boolean adminlog(int userid,String pass) throws SQLException {
		boolean res=false;
		String q="select * from admin where UserID=? and Password=?";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1, userid);
		pst.setString(2, pass);
		ResultSet rs=pst.executeQuery();
		while(rs.next()) {
			res=true;
		}
		return res;
	}
	//insert a data for new user into user table
	public boolean newuser(int userid,String pass) throws SQLException {
		String q="insert into users(UserID,Password) values(?,?)";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1, userid);
		pst.setString(2, pass);
		int r=pst.executeUpdate();
		return (r>0)?true:false;
	}
	//Display a Menu items
	public void menuitems() throws SQLException{
		String q="select * from menu_items";
		PreparedStatement pst=con.prepareStatement(q);
		ResultSet rs=pst.executeQuery();
		while(rs.next()) {
			System.out.println("MenuID:"+rs.getInt(1));
			System.out.println("Food Name:"+rs.getString(2));
			System.out.println("Price:"+rs.getFloat(3));
			System.out.println();
		}
	}
	//Add a food items in orders table
	public boolean additems(int userid,int menuid,String item,int quan) throws SQLException {
		String q="insert into orders(UserID,MenuId,ItemName,Quantity) values(?,?,?,?)";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1, userid);
		pst.setInt(2, menuid);
		pst.setString(3, item);
		pst.setInt(4,quan);
		int r=pst.executeUpdate();
		return (r>0)?true:false;
	}
	
	//show ordered items based on userid
	public void showordereditems(int userid) throws SQLException {
		String q="select orderid,MenuID,itemname,quantity from orders where userid=?";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1,userid);
		ResultSet rs=pst.executeQuery();
		while(rs.next()) {
			System.out.println("Order ID:"+rs.getInt("OrderID"));
			System.out.println("Menu ID:"+rs.getInt("MenuID"));
			System.out.println("Ordered Item:"+rs.getString("ItemName"));
			System.out.println("Quantity:"+rs.getInt("Quantity"));
			System.out.println();
		}
    }
	//to edit ordered items based on orderid and userid
	public boolean editorderitem(int menuid,String item,int quan,int orderid,int userid) throws SQLException{
		String q="update orders set MenuId=?,ItemName=?,Quantity=? where OrderID=? and UserID=?";
		PreparedStatement pst =con.prepareStatement(q);
		pst.setInt(1, menuid);
		pst.setString(2, item);
		pst.setInt(3, quan);
		pst.setInt(4, orderid);
		pst.setInt(5, userid);
		int r=pst.executeUpdate();
		return (r>0)?true:false;
	}
	//to delete ordered items based on orderid and userid
	public boolean deleteorderitem(int orderid,int userid) throws SQLException{
		String q="delete from orders where orderid=? and userid=?";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1, orderid);
		pst.setInt(2, userid);
		int r=pst.executeUpdate();
		return (r>0)?true:false;
	}
	//for admin user to show order details
	public void showorders() throws SQLException {
		String q="select * from orders";
		PreparedStatement pst=con.prepareStatement(q);
		ResultSet rs=pst.executeQuery();
		while(rs.next()) {
			System.out.println("Order ID:"+rs.getInt(1));
			System.out.println("User ID:"+rs.getInt(2));
			System.out.println("Menu ID:"+rs.getInt(3));
			System.out.println("Ordered Item:"+rs.getString(4));
			System.out.println("Quantity:"+rs.getInt(5));
			System.out.println();
		}
    }
	//for admin user to show customer details
	public void showcustomerdetails() throws SQLException {
		String q="select * from customer";
		PreparedStatement pst=con.prepareStatement(q);
		ResultSet rs=pst.executeQuery();
		while(rs.next()) {
			System.out.println("User ID:"+rs.getInt(1));
			System.out.println("Customer Name:"+rs.getString(2));
			System.out.println("Location:"+rs.getString(3));
			System.out.println("Customer Phoneno.:"+rs.getLong(4));
			System.out.println();
		}
		
	}
	//for admin insert a items in menu_table
	public boolean addmenuitems(int menuid,String item,int price) throws SQLException {
		String q="insert into menu_items values(?,?,?)";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1,menuid);
		pst.setString(2, item);
		pst.setInt(3,price);
		int r=pst.executeUpdate();
		return (r>0)?true:false;
	}
	//for admin edit a items in menu_table
	public boolean editmenuitem(int menuid,String item,int price) throws SQLException{
		String q="update menu_items set ItemName=?,Price=? where MenuID=?";
		PreparedStatement pst =con.prepareStatement(q);
		pst.setString(1, item);
		pst.setInt(2, price);
		pst.setInt(3, menuid);
		int r=pst.executeUpdate();
		return (r>0)?true:false;
	}
	//for admin delete items in menu_table
	public boolean deletemenuitem(int menuid) throws SQLException{
		String q="delete from menu_items where MenuID=?";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1, menuid);
		int r=pst.executeUpdate();
		return (r>0)?true:false;
	}
	//for admin to know confirm orders details
	public void confirmorderdetails() throws SQLException{
		String q="select * from confirm_orders";
		PreparedStatement pst=con.prepareStatement(q);
		ResultSet rs=pst.executeQuery();
		while(rs.next()) {
			System.out.println("OrderId:"+rs.getInt("orderid"));
			System.out.println("UserId:"+rs.getInt("Userid"));
			System.out.println("Total Amt:"+rs.getFloat("Total_amt"));
			System.out.println("Order Status:"+rs.getString("status"));
		}
		
	}
	//to add customer details
	public boolean addcusdeat(int userid,String cname,String loc,long phon) throws SQLException {
		String q="insert into Customer(UserID,Cname,location,phno) values(?,?,?,?)";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1, userid);
		pst.setString(2,cname);
		pst.setString(3,loc);
		pst.setLong(4, phon);
		int r=pst.executeUpdate();
		return (r>0)?true:false;
	}
	//summary for orders
	public float summary(int userid) throws SQLException{
		showcustomerdetails();
		System.out.println();
		int uid=userid;
		showordereditems(uid);
		System.out.println();
		int totamt=0;
		String q="select m.price,o.quantity from menu_items m inner join orders o on m.menuid=o.menuid where userid=?";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1, uid);
		ResultSet rs=pst.executeQuery();
		while(rs.next()) {
			float pr=rs.getFloat("price");
			int qu=rs.getInt("Quantity");
			float am=pr*qu;
			totamt+=am;
		}
		System.out.println("Total Payable Amount:"+totamt+"(All tax includs)");
		System.out.println("<-----------Pay a Amount---------->");
		return totamt;
		
		
	}
	public void confirm_order(int userid,float totalamt) throws SQLException{
		String 	q="insert into confirm_orders(userid,Total_amt,Status) values(?,?,'Paid')";
		PreparedStatement pst=con.prepareStatement(q);
		pst.setInt(1, userid);
		pst.setFloat(2, totalamt);
		int r=pst.executeUpdate();
		if(r>0) {
			System.out.println("*****Your Order Placed*****");
		}
	}
	//to exit
	public void exit() {
		System.out.println("Thank You for Booking With Us!!\nVisit Again");
		
	}	
}
...............................................................................................................................................................
//customer.java class
package fdmanage;
import java.sql.SQLException;
import java.util.InputMismatchException;
import java.util.Scanner;
public class Customer extends Foodordapp{
	Scanner sc=new Scanner(System.in);
	public Customer() throws SQLException,InputMismatchException{
		System.out.println("***********WELCOME TO ABC SUPERMARKET APPLICATION**********");
		System.out.print("Pls Enter (User/Admin):");
		String type=sc.next().toLowerCase();
		if(type.equals("admin")) {
			adminverify();
		}
		else if(type.equals("user")) {
			userverify();
		}
		else {
			System.out.println("Give Valid Detail");
		}
	}
	public void userverify() throws SQLException,InputMismatchException{
		System.out.print("1.Existing User\n2.New User\nEnter the Choice:");
		int ch=sc.nextInt();
		if(ch==1) {
			System.out.print("Enter the UserID:");
			int userid=sc.nextInt();
			System.out.print("Enter the Password:");
			String pass=sc.next();
			if(userlog(userid,pass)) {
				System.out.print("Are you Completed Filling Your Details?(yes/no):");
				String choice=sc.next().toLowerCase();
				if(choice.equals("yes")) {
					userdashboard();
					System.out.println("----------------------------------------------------------------------------------");
				}
				else {
					System.out.println("----------------------------------------------------------------------------------");
					customerdetails();
				}
				
			}
			else {
				System.out.println("Invalid Credential\nTry Again..");
			}
		}
		else {
			System.out.print("Create the UserID:");
			int userid=sc.nextInt();
			System.out.print("Create the Password:");
			String pass=sc.next();
			if(newuser(userid,pass)) {
				System.out.println("Id and Password Created...");
				userverify();
			}
		}
		
	}
	public void adminverify() throws SQLException,InputMismatchException{
		System.out.print("Enter the UserID:");
		int userid=sc.nextInt();
		System.out.print("Enter the Password:");
		String pass=sc.next();
		if(adminlog(userid,pass)) {
			admindashboard();
		}
		else {
			System.out.println("Invalid Credential\nTry Again..");
		}
		
	}
	public void admindashboard() throws SQLException,InputMismatchException{
		System.out.println("****Welcome to DashBoard****");
		while(true) {
			System.out.print("1.Show Orders\n2.Show Customers Details\n3.Show MenuItems\n4.Add MenuItems\n5.Edit MenuItems\n6.Delete MenuItems\n7.Confirm Orders List\n8.Exit\nEnter the Choice:");
			int ch=sc.nextInt();
			if(ch==1) {
				showorders();
				System.out.println("----------------------------------------------------------------------------------");
			}
			else if(ch==2) {
				showcustomerdetails();
				System.out.println("----------------------------------------------------------------------------------");
			}
			else if(ch==3) {
				menuitems();
				System.out.println("----------------------------------------------------------------------------------");
				
			}
			else if(ch==4) {
				System.out.print("Enter the MenuId:");
				int menuid=sc.nextInt();
				sc.nextLine();
				System.out.print("Enter the ItemName:");
				String item=sc.nextLine();
				System.out.print("Enter the Price:");
				int price=sc.nextInt();
				if(addmenuitems(menuid,item,price)) {
					System.out.println("Item Added...");
					System.out.println("----------------------------------------------------------------------------------");
				}
			}
			else if(ch==5) {
				System.out.print("Enter the MenuId:");
				int menuid=sc.nextInt();
				sc.nextLine();
				System.out.print("Enter the ItemName:");
				String item=sc.nextLine();
				System.out.print("Enter the Price:");
				int price=sc.nextInt();
				if(editmenuitem(menuid,item,price)) {
					System.out.println("Menu Item Updated...");
					System.out.println("----------------------------------------------------------------------------------");
				}
			}
			else if(ch==6) {
				System.out.print("Enter the MenuId:");
				int menuid=sc.nextInt();
				if(deletemenuitem(menuid)) {
					System.out.println("Menu Item Deleted...");
					System.out.println("----------------------------------------------------------------------------------");
				}
			}
			else if(ch==7) {
				confirmorderdetails();
				System.out.println("----------------------------------------------------------------------------------");
			}
			else {
				System.out.println("Thank You Visited");
				System.out.println("----------------------------------------------------------------------------------");
				break;
			}
		}
	}
	public void customerdetails() throws SQLException,InputMismatchException {
		System.out.print("Enter Your UserID:");
		int userid=sc.nextInt();
		sc.nextLine();
		System.out.print("Enter Your Name:");
		String cname=sc.nextLine();
		//sc.nextLine();
		System.out.print("Enter Your Location:");
		String loc=sc.nextLine();
		System.out.print("Enter Your Phone No.:");
		long phno=sc.nextLong();
		if(addcusdeat(userid,cname,loc,phno)) {
			System.out.println("Deatails Successfully Added...");
			System.out.println("----------------------------------------------------------------------------------");
			userdashboard();
		}
		else {
			System.out.println("Invalid Credential\nTry Again..");
		}
	}
	public void userdashboard() throws SQLException,InputMismatchException{
		System.out.println("**********Welcome to DashBoard************");
		while(true) {
			System.out.println("1.Menu Items\n2.Add Items\n3.Show Ordered Items\n4.Edit Items\n5.Delete Items\n6.Order Summary\n7.Exit\nEnter the Choice:");
			int ch=sc.nextInt();
			System.out.println();
			if(ch==1) {
				System.out.println("<----------------------------MENU ITEMS---------------------------->");
				menuitems();
				System.out.println("----------------------------------------------------------------------------------");
			}
			else if(ch==2) {
				System.out.print("Enter your UserID:");
				int userid=sc.nextInt();
				System.out.print("Enter the Menu ID:");
				int menuid=sc.nextInt();
				sc.nextLine();
				System.out.print("Enter the Item Name:");
				String item=sc.nextLine();
				System.out.print("Enter the Quantity:");
				int quan=sc.nextInt();
				if(additems(userid,menuid,item,quan)) {
					System.out.println("Item Added");
				}
				else {
					System.out.println("Item Not Added\nTry Again...");
				}
				System.out.println("----------------------------------------------------------------------------------");
				
			}
			else if(ch==3) {
				System.out.print("Enter your UserID:");
				int userid=sc.nextInt();
				showordereditems(userid);
				System.out.println("----------------------------------------------------------------------------------");
			}
			else if(ch==4) {
				System.out.print("Enter your UserID:");
				int userid=sc.nextInt();
				System.out.print("Enter the OrderID(if you know use 'Show Ordered'):");
				int orderid=sc.nextInt();
				sc.nextLine();
				System.out.print("Enter the Menu ID:");
				int menuid=sc.nextInt();
				sc.nextLine();
				System.out.print("Enter the Item Name:");
				String item=sc.nextLine();
				System.out.print("Enter the Quantity:");
				int quan=sc.nextInt();
				if(editorderitem(menuid,item,quan,orderid,userid)) {
					System.out.println("Update Succesfully");
				}
				else {
					System.out.println("Item not Updated\nTry Again...");
				}
				System.out.println("----------------------------------------------------------------------------------");
				
			}
			else if(ch==5) {
				System.out.print("Enter your UserID:");
				int userid=sc.nextInt();
				System.out.print("Enter the OrderID(if you know use 'Show Ordered'):");
				int orderid=sc.nextInt();
				if(deleteorderitem(orderid,userid)) {
					System.out.println("Item Deleted");
					System.out.println("----------------------------------------------------------------------------------");
				}
				else {
					System.out.println("Item not Deleted\nTry Again...");
				}
			}
			else if(ch==6) {
				System.out.println("Enter your UserID:");
				int userid=sc.nextInt();
				System.out.println("<------------------------ORDER SUMMARY---------------------->");
				float totamt=summary(userid);
				System.out.println("Are you Paid your amount(yes/no):");
				String choice=sc.next().toLowerCase();
				if(choice.equals("yes")) {
					confirm_order(userid,totamt);
					exit();
					break;
				}
				else {
					System.out.println("--Please Pay Your Amount");
				}
				
			}
			else if(ch==7) {
				exit();
				break;
			}
			else {
				System.out.println("Invalid Credential\nTry Again...");
			}
		}
	}
}


......................................................................................................................................................
//Main class

package fdmanage;
import java.sql.SQLException;
import java.util.InputMismatchException;
public class Main {
	public static void main(String[] args) throws InputMismatchException, SQLException {
		@SuppressWarnings("unused")
		Customer c1=new Customer();
	}

}
