   package com.book.dao;

import java.sql.Connection;


import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;

import com.book.model.bin;
import com.book.util.DBUtility;


public class randao implements rdao{

  public randao() {
		// initialization
	}
	
	
	
	public int insertbin(bin ob) {
		Connection conn = null;
		PreparedStatement pstat = null;
		Statement stat=null;
		ResultSet rs = null;
		int id = 0;
		try {
			conn = connectiondao.createConnection();
			stat= conn.createStatement();
			String query = "select seq_id.nextval from dual";
			rs = stat.executeQuery(query);
			while (rs.next()) {       
				id = rs.getInt(1);
			}
			
			System.out.println(id);
			
				
				pstat = conn
						.prepareStatement("insert into details(user_id,user_name,contact_no,address,email_id,password) values(?, ?, ?, ?, ?, ?)");

      
								
				pstat.setInt(1,id );
				pstat.setString(2, ob.getName());
				pstat.setString(3, ob.getContact_no());
				pstat.setString(4, ob.getAddress());
				pstat.setString(5, ob.getEmail());
				pstat.setString(6, ob.getPassword());
			

				 pstat.executeUpdate();
				
				
			} catch (SQLException e) {
				e.printStackTrace();
			} catch (Exception e) {
				e.printStackTrace();
			} finally {
				DBUtility.close(rs);
				DBUtility.close(pstat);
				DBUtility.close(conn);
			}
		return id;
			
		}
	public bin findbin(int a) {
		// 
		Connection conn = null;
		Statement stat = null;
		ResultSet rs = null;
        bin b1=new bin();
		try {
			conn = connectiondao.createConnection();
			stat = conn.createStatement();

			String query = "select * from details where user_id='"
					+ a + "'";
			rs = stat.executeQuery(query);
			if (rs.next()) {
				b1.setId(rs.getInt(1));
				b1.setName(rs.getString(2));
				b1.setContact_no(rs.getString(3));
				b1.setAddress(rs.getString(4));
				b1.setEmail(rs.getString(5));
				b1.setPassword(rs.getString(6));
			
				
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} catch (Exception e) {
			e.getMessage();
		} finally {
			DBUtility.close(conn);
			DBUtility.close(stat);
			DBUtility.close(rs);
		}
		return b1;
	}
	public void updatebin(bin ob1) {
		// 
		Connection conn = null;
		Statement stat = null;
		ResultSet rs = null;
                PreparedStatement pstat = null;
		try {
conn = connectiondao.createConnection();
			
                           pstat = conn
					.prepareStatement("UPDATE details SET user_name=?, address=?, contact_no=?, email_id=? where user_id='"
							+ ob1.getId() + "'");
			pstat.setString(1, ob1.getName());
			pstat.setString(2, ob1.getAddress());
			pstat.setString(3, ob1.getContact_no());
			pstat.setString(4, ob1.getEmail());
      
								
		    pstat.executeUpdate();
		    
				
			} catch (SQLException e) {
				e.printStackTrace();
			} catch (Exception e) {
				e.printStackTrace();
			} finally {
				DBUtility.close(rs);
				DBUtility.close(pstat);
				DBUtility.close(conn);
			}
	}

	    public int deletebin(int ad) {
	    	
	    	Connection conn = null;
			Statement stat = null;
			ResultSet rs = null;
            int admid= ad;
			try {
				conn = connectiondao.createConnection();
				stat = conn.createStatement();
                 
				String query = "delete from details where user_id='"
						+ ad + "'";
				stat.executeQuery(query);
				
			} catch (SQLException e) {
				e.printStackTrace();
			} catch (Exception e) {
				e.getMessage();
			} finally {
				DBUtility.close(conn);
				DBUtility.close(stat);
				DBUtility.close(rs);
			}
			return admid;	
	    }







package com.book.dao;

import java.sql.DriverManager;


import java.sql.SQLException;

import com.book.dao.*;



public class connectiondao
{
  
	
	
	
	

	
	
	public static final String DRIVER = "oracle.jdbc.driver.OracleDriver";

	public static final String DBURL = "jdbc:oracle:thin:@172.24.137.30:1521:ora10g";

	public static final String DBUSER = "e595393"; 

	public static final String DBPASS = "ODivjlfbj";
	
	public static java.sql.Connection createConnection() {
		// Use DRIVER and DBURL to create a connection
		// Recommend connection pool implementation/usage
		java.sql.Connection conn=null;
		try {

			Class.forName("oracle.jdbc.driver.OracleDriver");

			conn = DriverManager.getConnection(DBURL,DBUSER,DBPASS);

			System.out.println("DATABASE CONNECTION ESTABLISHED!n\n");

		}

		catch (ClassNotFoundException ce) {

			System.out
					.println("Sorry error in loading class.....Class not found.");
		}

		catch (SQLException se) {

			System.out
					.println("\t!!!!!!!!!!DATABASE CONNECTION IS NOT ESTABLISHED!!!!!!!!!!!\n\n");
		}

		return conn;
	}

	public rdao getrdao() {
		
		return new randao();
	}
}













               package com.book.servlet;

import java.io.IOException;

import java.io.PrintWriter;
import java.util.ArrayList;
import java.sql.Date;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.book.dao.rdao;

import com.book.dao.connectiondao;
import com.book.model.bin;


public class randomservlet extends HttpServlet {
  private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public randomservlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
	
    }
	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */

protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
{
		
	connectiondao cd = new connectiondao();
	rdao r =cd.getrdao();
		
		bin b = new bin();
		
		String page = request.getParameter("CheckPage");
		System.out.println("selected Page :::: " + page);
		
		if (page.equals("add"))
		{
			int a;
			b.setName(request.getParameter("Name"));
			b.setAddress(request.getParameter("Address"));
			b.setEmail(request.getParameter("EmailAddress"));
			b.setPassword(request.getParameter("password"));
			b.setContact_no(request.getParameter("PhoneNO"));
		
			a=r.insertbin(b);
			
			PrintWriter out = response.getWriter();
			
			out.println("<fieldset  style=\"width: 800px; height: 80px;\">DETAILS ADDED SUCCESSFULLY.... GENERATED ID = "+a);
			out.println("</fieldset>");
		}
		if (page.equals("display"))
		{
			int id = Integer.parseInt(request.getParameter("ID"));
			
			b=r.findbin(id);
			if(b.getId()==0)
			{
			PrintWriter out = response.getWriter();
			out.println("<fieldset  style=\"width: 800px; height: 80px;\">WRONG ID PROVIDED");
			out.println("</fieldset>");
			}
			else
			{
				PrintWriter out = response.getWriter();
			out.println("details of  "+ "id = "+id);
			out.println("<html><head></head>");
			out.println("<body><table align= \"center\"><tr bgcolor=\"#2E9AFE\"><th align=\"center\">ID</th> <th align=\"center\">NAME</th> <th align=\"center\">ADDRESS</th> <th align=\"center\">CONTACTNO</th> <th align=\"center\">EMAILID</th> </tr>");
			out.println("<tr bgcolor=\"#81DAF5\"><td align= \"center\">"+ b.getId()+"</td>"+"<td align= \"center\">"+b.getName()+"</td>"+"<td align= \"center\">"+b.getAddress()+"</td>"+"<td align= \"center\">"+b.getContact_no()+"</td>"+"<td align= \"center\">"+b.getEmail()+"</td>"+"</tr>");
			out.println("</table> <br><br> <p></body></html>");
			}
		}
		
		if (page.equals("update"))
		{
			int id = Integer.parseInt(request.getParameter("ID"));
			
			b=r.findbin(id);
			if(b.getId()==0)
			{
				PrintWriter out = response.getWriter();
				out.println("<fieldset  style=\"width: 800px; height: 80px;\">WRONG ID PROVIDED");
				out.println("</fieldset>");
			}
			else
			{
			request.setAttribute("upd_obj", b);
			
				RequestDispatcher rd = request.getRequestDispatcher("/jsp/update1.jsp");
			rd.forward(request, response);
			}
		}
		if (page.equals("update1"))
		{
			int a;
			b.setId(Integer.parseInt(request.getParameter("id")));
			b.setName(request.getParameter("name"));
			b.setAddress(request.getParameter("address"));
			b.setEmail(request.getParameter("email"));
			b.setPassword(request.getParameter("password"));
			b.setContact_no(request.getParameter("contact_no"));
		
			r.updatebin(b);
			
			PrintWriter out = response.getWriter();
			out.println("<fieldset  style=\"width: 800px; height: 80px;\">DETAILS UPDATED SUCCESSFULLY.... ");
			out.println("</fieldset>");
			
			
		}
		if (page.equals("delete"))
		{
			int id = Integer.parseInt(request.getParameter("ID"));
			b=r.findbin(id);
			if(b.getId()==0)
			{
				PrintWriter out = response.getWriter();
				out.println("<fieldset  style=\"width: 800px; height: 80px;\">WRONG ID PROVIDED");
				out.println("</fieldset>");
			}
			else
			{
			int am= r.deletebin(id);
			
			PrintWriter out = response.getWriter();
			out.println("<fieldset  style=\"width: 800px; height: 80px;\">deleted details of  "+ "id = "+am);
			out.println("</fieldset>");
		}
		}
}
}







package com.book.util;

import java.sql.*;
public class DBUtility {

  public static void close(Connection conn) {
		try {
			if (!conn.isClosed()) {
				conn.close();
			}
		} catch (Exception e) {
		}
	}

	public static void close(Statement stat) {
		try {

			stat.close();

		} catch (Exception e) {
		}
	}

	public static void close(PreparedStatement prestat) {
		try {

			prestat.close();

		} catch (Exception e) {
		}
	}

	public static void close(ResultSet rs) {
		try {

			rs.close();

		} catch (Exception e) {
		}
	}

}







<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<script src="<%= request.getContextPath() %>/js/create.js"></script>

<link rel="stylesheet" type="text/css" href="<%= request.getContextPath() %>/css/style.css" />

</head>

<body background="<%=request.getContextPath()%>/images/bgcolor.png ">
<div style="width:600px;height:250px;padding-left:150px;padding-top:120px;">

<fieldset style="width:450px;padding-left:30px;">

<legend ><b>Add</b></legend>

<form name="Create" method="post" action="<%= request.getContextPath() %>/randomservlet" onsubmit="return validateCreate()">
<font color="red">All fields are mandatory!</font> 

<table>
<tr>
<td >Name : 
</td>
<td>
<input type="text" name="Name" value="" size="25">
</td>
</tr>



<tr>
<td >Address : 
</td>
<td><textarea name="Address"value="" cols="20" rows="3"></textarea>
</td>
<br>
</td>
</tr>


<tr>
<td >Phone No :
</td>
<td> <input type="text" name="PhoneNO" value="" size="25">
<br>
</td>
</tr>



<tr>
<td >E-mail Address :
 </td>
<td>
<input type="text" name="EmailAddress" value="" size="25">
<br>
</td>
</tr>

<tr>
<td >Password :
</td>
<td>
<input type ="password" name="password" value="" size="15">
<br>
</td>
</tr>



<tr>
</table>

<br><br>

<table align="centre">

<tr>
<td>
<b><input class="addbtn" type="submit" value="Submit"></td><td><input class="resetbtn" type="reset" value="Reset">
<input type = "hidden" name= "CheckPage" value="add">
</td>
</tr>
</table>
</tr>

</table>

</form> 
</fieldset>
</div>
</body>
</html>









<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Insert title here</title>
</head>
<body>

<td><a href="add.jsp "/a>Add</a></td><br>
<td><a href="view.jsp "/a>view</a></td><br>
<td><a href="update.jsp "/a>update</a></td><br>
<td><a href="delete.jsp "/a>delete</a></td><br>
</body>
</html>






<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<script src="<%= request.getContextPath() %>/js/updatead.js"></script>

<link rel="stylesheet" type="text/css" href="<%= request.getContextPath() %>/css/style.css" />

</head>

<body background="<%=request.getContextPath()%>/images/bgcolor.png ">
<div style="width:600px;height:250px;padding-left:150px;padding-top:120px;">

<fieldset style="width:450px;padding-left:30px;">

<legend ><b>Update</b></legend>

<form name="Update" method="post" action="<%= request.getContextPath() %>/randomservlet" onsubmit="return validateUpdate()">
<font color="red">All fields are mandatory!</font> 
<table>


<tr>
<td >ID *:
 </td>
<td><input type="text" name="ID" value="" size="25">
<br>
</td>
</tr>



</table>
<br><br>

<table align="centre">
<tr>
<td>
<b><input class="addbtn" type="submit" value="Enter">
</td>
<td><input class="resetbtn" type="reset" value="Reset">
</td>
<input type = "hidden" name= "CheckPage" value="update">
</tr>

</table>
</tr>
</table>
</form> 

</fieldset>
</body>
</html>






<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1" import="com.book.model.bin"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<script src="<%= request.getContextPath() %>/js/updatead.js"></script>
<title>Insert title here</title>
</head>
<body background="<%=request.getContextPath()%>/images/bgcolor.png ">
<div style="width:600px;height:250px;padding-left:150px;padding-top:120px;">
<h1>UPDATE ADMIN DETAILS</h1>
<% bin b = (bin)request.getAttribute("upd_obj"); %>
<form  name="Create" method="post" action="<%=request.getContextPath()%>/randomservlet" onsubmit="return validateCreate()">

<font color="red">All fields are mandatory!</font> 
<input type="hidden" name="id" value="<%=b.getId() %>">
<table>
<tr><td>NAME * : </td><td><input type="text" name="name" value="<%=b.getName() %>"></td></tr>
<tr><td>ADDRESS *: </td><td><textarea name="address"  cols="20" rows="3" ><%=b.getAddress() %></textarea></td></tr>
<tr><td>CONTACT NUMBER * : </td><td> <input type="text" name="contact_no" value="<%=b.getContact_no() %>"></td></tr>
<tr><td>EMAIL ID * :  </td><td><input type="text" name="email" value="<%=b.getEmail() %>"></td></tr>

</table>
<table>
<tr><td><input type="submit" value="update"></td>
<td><input type="reset" value="reset"></td></tr></table>
<input type="hidden" name="CheckPage" value="update1">
</form>

</div>
</body>
</html>


		

}





