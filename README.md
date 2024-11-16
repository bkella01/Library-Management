import java.sql.*;
import java.util.Vector;
import java.util.logging.Level;
import java.util.logging.Logger;
import javax.swing.JOptionPane;
import javax.swing.table.DefaultTableModel;



public StudentJFrame() {
        initComponents();
        Connect();
        StudentData();
    }

    Connection con;
    PreparedStatement pst;


    public void Connect(){

        try {
            Class.forName("com.mysql.jdbc.Driver");
            con = DriverManager.getConnection("jdbc:mysql://localhost/librarydb","root","");
        } catch (ClassNotFoundException ex) {
            Logger.getLogger(StudentJFrame.class.getName()).log(Level.SEVERE, null, ex);
        } catch (SQLException ex) {
            Logger.getLogger(StudentJFrame.class.getName()).log(Level.SEVERE, null, ex);
        }





    }




   private void StudentData(){

        try {
            int QQ;
            pst = con.prepareStatement("SELECT * FROM student");
            ResultSet Rs = pst.executeQuery();

            ResultSetMetaData RSMD = Rs.getMetaData();

            QQ = RSMD.getColumnCount();

            DefaultTableModel DFG =(DefaultTableModel)table1.getModel(); 

            DFG.setRowCount(0);

            while(Rs.next()){

            Vector v2 = new Vector();

            for(int aa=1; aa<=QQ; aa++){

                v2.add(Rs.getString("studentid"));
                v2.add(Rs.getString("studentname"));
                v2.add(Rs.getString("email"));
                v2.add(Rs.getString("address"));
             }

             DFG.addRow(v2);

        }

        } catch (SQLException ex) {
            Logger.getLogger(StudentJFrame.class.getName()).log(Level.SEVERE, null, ex);
        }


    }





   Insert Code



   try {
            String studentid = txtId.getText();
            String studentname = txtName.getText();
            String email = txtEmail.getText();
            String address = txtAddress.getText();

            pst = con.prepareStatement("INSERT INTO student (studentid,studentname,email,address)VALUES(?,?,?,?)");

            pst.setString(1,studentid);
            pst.setString(2,studentname);
            pst.setString(3,email);
            pst.setString(4,address);

            pst.executeUpdate();
            JOptionPane.showMessageDialog(this, "Record Inserted Successfully");
            StudentData();
        } catch (SQLException ex) {
            Logger.getLogger(StudentJFrame.class.getName()).log(Level.SEVERE, null, ex);
        }
    }                                        






   Update Code


   try {
            String studentid = txtId.getText();
            String studentname = txtName.getText();
            String email = txtEmail.getText();
            String address = txtAddress.getText();

            pst = con.prepareStatement("update student set studentname= ?,email= ?,address= ? where studentid= ?");


            pst.setString(1,studentname);
            pst.setString(2,email);
            pst.setString(3,address);
            pst.setString(4,studentid);

            pst.executeUpdate();
            JOptionPane.showMessageDialog(this, "Record Updated Successfully");
            StudentData();

        } catch (SQLException ex) {
            Logger.getLogger(StudentJFrame.class.getName()).log(Level.SEVERE, null, ex);
        }
    }                                  






   Delete Code


   try {
            String studentid = txtId.getText();
            pst=con.prepareStatement("DELETE FROM student WHERE studentid=?");
            pst.setString(1,studentid);

            pst.executeUpdate();
            JOptionPane.showMessageDialog(this, "Record Deleted Successfully");
            StudentData();

        } catch (SQLException ex) {
            Logger.getLogger(StudentJFrame.class.getName()).log(Level.SEVERE, null, ex);
        }

    }                         






  Btn total Code


  try {
            String sql="select count(studentname) from student";
            pst=con.prepareStatement(sql);
            ResultSet Rs = pst.executeQuery();
            if(Rs.next()){
            String sum=Rs.getString("count(studentname)");
            txtSum1.setText(sum);
            }
        } catch (SQLException ex) {
            Logger.getLogger(StudentJFrame.class.getName()).log(Level.SEVERE, null, ex);
        }
    }                       







  SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            String date_borrowed = sdf.format(DateChooser1.getDate());
            String date_return = sdf.format(DateChooser2.getDate());
