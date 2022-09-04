# JDBC
Java Database connectivity perform and using Mysql and java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>JDBC</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.29</version>
        </dependency>

    </dependencies>

    <properties>
        <maven.compiler.source>18</maven.compiler.source>
        <maven.compiler.target>18</maven.compiler.target>
    </properties>


package db;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class MyConnection {
 static   Connection connection = null;
 public static Connection getConnection() {
     try {
         Class.forName("com.mysql.cj.jdbc.Driver");
         connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/JDBC", "root", "sanskar");
         System.out.println("connection ho gya");
     }catch (ClassNotFoundException ex) {
         ex.printStackTrace();
     } catch (SQLException ex) {
         ex.printStackTrace();
     }
     return connection;
 }


 public static void closeConnection(){

 }


    }

package model;

public class User {
  private  int id;
  private  String name;
  private  String email;


    public User(int id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;

    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}

  package dao;

import db.MyConnection;
import model.User;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

public class UserDAO {
    public static List<User> getAllUsers() throws SQLException {
        List<User> users = new ArrayList<>();
        Connection conn = MyConnection.getConnection();
        PreparedStatement ps = conn.prepareStatement("select * from user");
        ResultSet rs = ps.executeQuery();
        while (rs.next()) {
            int id = rs.getInt(1);
            String name = rs.getString(2);
            String email = rs.getString(3);
            User u = new User(id, name, email);
            users.add(u);
        }
        return users;
    }

    public static boolean deleteUser(int id) throws SQLException {
        User user = null;
        Connection conn = MyConnection.getConnection();
        PreparedStatement ps = conn.prepareStatement("select * from user where id = ?");
        ps.setInt(1, id);
        ResultSet rs = ps.executeQuery();
        while (rs.next()) {
            id = rs.getInt(1);
            String name = rs.getString(2);
            String email = rs.getString(3);
            user = new User(id, name, email);

        }

        return user;
    }


    public static boolean saveUser(User user) throws SQLException {
        Connection conn = MyConnection.getConnection();
        PreparedStatement ps = conn.prepareStatement("insert into user  values(default, ?,?");
        ps.setString(1, user.getName());
        ps.setString(2, user.getEmail());
        int ans = ps.executeUpdate();
        return ans == 1;


    }
    public static boolean updateUser(User user) throws SQLException {
        Connection conn = MyConnection.getConnection();
        PreparedStatement ps = conn.prepareStatement( "update user set name =?, email =? where id =2");
        ps.setString(1, user.getName());
        ps.setString(2, user.getEmail());
        ps.setInt(3, user.getId());
        int ans = ps.executeUpdate();
        return ans == 1;
    }

    public static boolean deleteUser(User id) throws SQLException {
        Connection conn = MyConnection.getConnection();
        PreparedStatement ps = conn.prepareStatement(" delete from user where id = ?");
        ps.setInt(2, id);
        int ans = ps.executeUpdate();
        return ans == 1;

    }
}
package controller;

import dao.UserDAO;
import model.User;

import java.sql.SQLException;
import java.util.List;


public class App {
    // CRUD - Create , Read , Update , Delete
    public static void main(String[] args) {
        try {
            // List<User> users = UserDAO.getAllUsers();
            // for(User u: users) {
           // System.out.print.ln(u.getId() + "\t" + u.getname());
    //    }
      //  User u =    UserDAO.getUserById(1);
          //  System.out.println(u== null ? "Not found" : u);
            //Create
    //    User user = new User(0,"saksham","saksham@yahoo.com");
   //  boolean ans =   UserDAO.saveUser(user);
    //    System.out.println(ans ? "Inserted" : "gadbad ho gyi");
   //   boolean ans = UserDAO.deleteUser(2);
          //  System.out.println(ans);
            User user = new User(0,"kunal bhai","kunal@yahoo.com");
   boolean ans =         UserDAO.updateUser(user);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
