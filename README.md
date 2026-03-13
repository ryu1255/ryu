import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.*;
import java.util.ArrayList;
import java.util.Objects;




public class cashier_clean extends JFrame{




   // === For LOG-IN===
   JTextField txtUser;
   JPasswordField txtPass;
   JButton login_btn;




   // == for log in attempts ==
   int attempts = 0;






   // === For Main Menu ===
   JButton btn_manage, btn_sales, btn_logout, btn_exit, btn_cashier;


   // == Validation ==
   String current_user = "";




   // == For Manage ===
   JTextField prod_id_txt, prod_name_txt, quan_txt, price_txt, total_value_txt;
   JButton m_computebtn, m_btnSave, m_btnDelete, m_btnSearch, m_btnPrevious, m_btnNext, m_btnClear;




   // == For Sales ===
   JTextField s_prod_id_txt, s_prod_name_txt, s_avail_quan_txt, s_quan_sold_txt, s_price_txt, s_total_sale_txt;
   JButton s_btnLoad, s_btnCompute, s_btnSave, s_clear, s_btntotalSales;
   int total_sales = 0, total_quan_sold = 0;




   // == For cashier ==
   private JTextField txtID, txtItem, txtPrice, txtQty, txtCash;
   private JLabel lblTotal, lblChange;
   private DefaultTableModel model;
   private JTable table;
   private double total = 0.0;
   int quantity_from_inventory;




   private ArrayList<String> inventory = new ArrayList<>();
   private int currentIndex = -1;








   public cashier_clean(){
       setTitle("Sales and Inventory Management System (Java Swing)");
       setSize(550,200);
       setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
       setLocationRelativeTo(null);




       // Init LOG-In
       txtUser = new JTextField(20);
       txtPass = new JPasswordField(20);
       btn_exit = new JButton("EXIT");
       login_btn = new JButton("LOG IN");




       // Panel
       JPanel panel = new JPanel(new GridLayout(3,2,12,12));
       panel.setBorder(BorderFactory.createEmptyBorder(20,20,20,20));




       //Extra UI
       JLabel title = new JLabel("Sales & Inventory System", SwingConstants.CENTER);
       title.setFont(new Font("Arial", Font.BOLD, 20));
       panel.setBackground(new Color(162, 207, 218));
       title.setForeground(new Color(255, 255, 255));




       // LOG-IN panels
       panel.add(new JLabel("Username: "));
       panel.add(txtUser);




       panel.add(new JLabel("Password: "));
       panel.add(txtPass);




       panel.add(btn_exit);
       panel.add(login_btn);




       add(panel);




       // Field UIS
       txtUser.setFont(new Font("Arial", Font.PLAIN, 14));
       txtPass.setFont(new Font("Arial", Font.PLAIN, 14));




       // Button UIS
       login_btn.addActionListener(e -> log_in());
       login_btn.setFont(new Font("Arial", Font.BOLD, 20));
       login_btn.setBackground(new Color(129, 210, 161));
       login_btn.setForeground(new Color(253, 253, 253));




       btn_exit.addActionListener(e -> System.exit(0));
       btn_exit.setFont(new Font("Arial", Font.BOLD, 20));
       btn_exit.setBackground(new Color(255, 100, 100));
       btn_exit.setForeground(Color.WHITE);
   }




   private void log_in(){
       if(attempts >= 3){
           JOptionPane.showMessageDialog(this, "Log In Terminated. Too many failed attempts.");
           System.exit(0);
           //return;
       }




       try{
           String username = txtUser.getText();
           String password = new String(txtPass.getPassword());




           String super_admin = "SuperAdmin";
           String sa_pass = "1234";


           String cashier = "Cashier";
           String ca_pass = "CA1234";


           String inventory_analyst = "InventoryAnalyst";
           String ia_pass = "IA1234";


           String salesman = "Saleman";
           String s_pass = "S1234";


//            if(attempts < 3){
//                if((password.equals(sa_pass) || password.equals(ca_pass) || password.equals(ia_pass) || password.equals(s_pass))  &&
//                        username.equals(super_admin) || username.equals(cashier) || username.equals(inventory_analyst) || username.equals(salesman)){
//                    btn_cashier.setEnabled(username.equals(cashier));
//                    btn_sales.setEnabled(username.equals(cashier));
//                    btn_manage.setEnabled(username.equals(inventory_analyst));
//                }else{
//                    JOptionPane.showMessageDialog(this, "Wrong credentials, try again");
//                    attempts++;
//                    return;
//                }
//            }else{
//                JOptionPane.showMessageDialog(this,
//                        "Log In Terminated. Too many failed attempts.",
//                        "Terminated",
//                        JOptionPane.ERROR_MESSAGE);
//                login_btn.setEnabled(false);
//                txtUser.setEnabled(false);
//                txtPass.setEnabled(false);
//            }








//


           if(attempts >= 0 && attempts < 3){
               if(username.equals(super_admin) && password.equals(sa_pass)){
                   attempts = 0;
                   JOptionPane.showMessageDialog(null, "Access Granted!", "Welcome", JOptionPane.INFORMATION_MESSAGE);
                   new Main_menu();
                   current_user = super_admin;
                   this.setVisible(false);
               }
               else if (username.equals(cashier) && password.equals(ca_pass)) {
                   attempts = 0;
                   JOptionPane.showMessageDialog(null, "Access Granted!", "Welcome", JOptionPane.INFORMATION_MESSAGE);
                   new Main_menu();
                   this.setVisible(false);
                   current_user = cashier;
                   btn_manage.setEnabled(false);
                   btn_sales.setEnabled(false);
               } else if (username.equals(inventory_analyst) && password.equals(ia_pass)) {
                   attempts = 0;
                   JOptionPane.showMessageDialog(null, "Access Granted!", "Welcome", JOptionPane.INFORMATION_MESSAGE);
                   new Main_menu();
                   this.setVisible(false);
                   current_user = inventory_analyst;
                   btn_cashier.setEnabled(false);
                   btn_sales.setEnabled(false);
               } else if (username.equals(salesman) && password.equals(s_pass)) {
                   attempts = 0;
                   JOptionPane.showMessageDialog(null, "Access Granted!", "Welcome", JOptionPane.INFORMATION_MESSAGE);
                   new Main_menu();
                   this.setVisible(false);
                   current_user = salesman;
                   btn_cashier.setEnabled(false);
                   btn_manage.setEnabled(false);
               }else{
                   attempts++;
                   int remaining = 3 - attempts;




                   if(remaining > 0){
                       JOptionPane.showMessageDialog(this,
                               "Incorrect credentials. " + remaining + " attempt(s) remaining.",
                               "Access Denied",
                               JOptionPane.ERROR_MESSAGE);
                   } else {
                       JOptionPane.showMessageDialog(this,
                               "Log In Terminated. Too many failed attempts.",
                               "Terminated",
                               JOptionPane.ERROR_MESSAGE);
                       login_btn.setEnabled(false);
                       txtUser.setEnabled(false);
                       txtPass.setEnabled(false);
                       System.exit(0);
                   }






                   txtUser.setText("");
                   txtPass.setText("");
               }


           }
//            else if (attempts == 3) {
//                JOptionPane.showMessageDialog(this,
//                        "Log In Terminated. Too many failed attempts.",
//                        "Terminated",
//                        JOptionPane.ERROR_MESSAGE);
//                login_btn.setEnabled(false);
//                txtUser.setEnabled(false);
//                txtPass.setEnabled(false);
//                }




       }catch(Exception e1){
           JOptionPane.showMessageDialog(null, "Access Denied", "Incorrect Username or Password", JOptionPane.ERROR_MESSAGE);
       }


   }




   // +++ MAIN MENU +++
   class Main_menu extends JFrame{




       Main_menu(){
           setTitle(" Sales and Inventory Management System (Java Swing)");
           setSize(400,300);
           setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
           setLocationRelativeTo(null);




           // Panel - FIX: was GridLayout(3,1) but 4 buttons were added
           JPanel panel = new JPanel(new GridLayout(4,1,12,12));
           panel.setBorder(BorderFactory.createEmptyBorder(20,20,20,20));




           btn_manage = new JButton("Manage Inventory");
           panel.add(btn_manage);
           btn_manage.setFont(new Font("Arial", Font.BOLD, 20));
           btn_manage.setBackground(new Color(118, 145, 246));
           btn_manage.setForeground(new Color(253, 253, 253));




           btn_sales = new JButton("Process Sales");
           panel.add(btn_sales);
           btn_sales.setFont(new Font("Arial", Font.BOLD, 20));
           btn_sales.setBackground(new Color(246, 220, 85));
           btn_sales.setForeground(new Color(253, 253, 253));




           btn_cashier = new JButton("Cashier");
           panel.add(btn_cashier);
           btn_cashier.setFont(new Font("Arial", Font.BOLD, 20));
           btn_cashier.setBackground(new Color(228, 161, 255));
           btn_cashier.setForeground(new Color(253, 253, 253));




           btn_logout = new JButton("Log Out");
           panel.add(btn_logout);
           btn_logout.setFont(new Font("Arial", Font.BOLD, 20));
           btn_logout.setBackground(new Color(255, 166, 166));
           btn_logout.setForeground(new Color(253, 253, 253));




           add(panel);
           panel.setBackground(new Color(162, 207, 218));




           btn_manage.addActionListener(new ActionListener() {
               @Override
               public void actionPerformed(ActionEvent e) {
                   new manage(Main_menu.this);
                   dispose();
               }
           });




           btn_sales.addActionListener(new ActionListener() {
               @Override
               public void actionPerformed(ActionEvent e) {
                   new sales(Main_menu.this);
                   dispose();
               }
           });




           btn_logout.addActionListener(new ActionListener() {
               @Override
               public void actionPerformed(ActionEvent e) {
                   new cashier_clean().setVisible(true);
                   dispose();
               }
           });




           btn_cashier.addActionListener(new ActionListener() {
               @Override
               public void actionPerformed(ActionEvent e) {
                   new cashier().setVisible(true);
                   dispose();
               }
           });




           setVisible(true);
       }
   }








   // +++ MANAGE +++
   public class manage extends JFrame{




       JFrame previousFrame;




       public manage(JFrame prev){
           this.previousFrame = prev;




           setTitle("Manage Inventory");
           setSize(1000, 550);
           setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
           setLocationRelativeTo(null);
           setLayout(new BorderLayout());




           prod_id_txt = new JTextField(20);
           prod_name_txt = new JTextField(20);
           quan_txt = new JTextField(20);
           price_txt = new JTextField(20);
           total_value_txt = new JTextField(20);
           total_value_txt.setEditable(false);




           m_computebtn = new JButton("Compute Total");
           m_btnSave = new JButton("Save");
           m_btnDelete = new JButton("Delete");
           m_btnDelete.setEnabled(!current_user.equals("InventoryAnalyst"));
           m_btnSearch = new JButton("Search");
           m_btnPrevious = new JButton("Previous");
           m_btnNext = new JButton("Next");
           m_btnClear = new JButton("Clear");




           JPanel titlePanel = new JPanel();
           titlePanel.setBackground(new Color(162, 207, 218));
           JLabel title = new JLabel("Inventory Management", SwingConstants.CENTER);
           title.setFont(new Font("Arial", Font.BOLD, 30));
           title.setForeground(Color.WHITE);
           title.setBorder(BorderFactory.createEmptyBorder(15, 0, 15, 0));
           titlePanel.add(title);




           JPanel fieldsPanel = new JPanel(new GridLayout(5, 2, 10, 10));
           fieldsPanel.setBorder(BorderFactory.createEmptyBorder(10, 40, 10, 40));
           fieldsPanel.setBackground(new Color(162, 207, 218));




           fieldsPanel.add(new JLabel("Product ID:"));
           prod_id_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(prod_id_txt);




           fieldsPanel.add(new JLabel("Product Name:"));
           prod_name_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(prod_name_txt);




           fieldsPanel.add(new JLabel("Product Quantity:"));
           quan_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(quan_txt);




           fieldsPanel.add(new JLabel("Price:"));
           price_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(price_txt);




           fieldsPanel.add(new JLabel("Total Value:"));
           total_value_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(total_value_txt);




           JPanel btnPanel = new JPanel(new GridLayout(3, 3, 10, 10));
           btnPanel.setBorder(BorderFactory.createEmptyBorder(10, 40, 20, 40));
           btnPanel.setBackground(new Color(162, 207, 218));




           btnPanel.add(m_computebtn);
           m_computebtn.setFont(new Font("Arial", Font.BOLD, 20));
           m_computebtn.setBackground(new Color(163, 189, 255));
           m_computebtn.setForeground(new Color(253, 253, 253));




           btnPanel.add(m_btnSave);
           m_btnSave.setFont(new Font("Arial", Font.BOLD, 20));
           m_btnSave.setBackground(new Color(116, 222, 114));
           m_btnSave.setForeground(new Color(253, 253, 253));




           btnPanel.add(m_btnDelete);
           m_btnDelete.setFont(new Font("Arial", Font.BOLD, 20));
           m_btnDelete.setBackground(new Color(255, 163, 163));
           m_btnDelete.setForeground(new Color(253, 253, 253));




           btnPanel.add(m_btnSearch);
           m_btnSearch.setFont(new Font("Arial", Font.BOLD, 20));
           m_btnSearch.setBackground(new Color(255, 229, 105));
           m_btnSearch.setForeground(new Color(253, 253, 253));




           btnPanel.add(m_btnPrevious);
           m_btnPrevious.setFont(new Font("Arial", Font.BOLD, 20));
           m_btnPrevious.setBackground(new Color(136, 153, 255));
           m_btnPrevious.setForeground(new Color(253, 253, 253));




           btnPanel.add(m_btnNext);
           m_btnNext.setFont(new Font("Arial", Font.BOLD, 20));
           m_btnNext.setBackground(new Color(136, 153, 255));
           m_btnNext.setForeground(new Color(253, 253, 253));




           btnPanel.add(m_btnClear);
           m_btnClear.setFont(new Font("Arial", Font.BOLD, 20));
           m_btnClear.setBackground(new Color(126, 126, 126));
           m_btnClear.setForeground(new Color(253, 253, 253));




           btnPanel.add(new JLabel(""));




           add(fieldsPanel, BorderLayout.CENTER);
           add(btnPanel, BorderLayout.SOUTH);
           add(titlePanel, BorderLayout.NORTH);




           m_btnSave.addActionListener(e -> m_save());
           m_btnDelete.addActionListener(e -> m_delete());
           m_btnSearch.addActionListener(e -> m_search());




           m_btnPrevious.addActionListener(e -> {
               if (currentIndex > 0) {
                   currentIndex--;
                   m_displayRecord(currentIndex);
               }
           });




           m_btnNext.addActionListener(e -> {
               if (currentIndex < inventory.size() - 1) {
                   currentIndex++;
                   m_displayRecord(currentIndex);
               }
           });




           m_btnClear.addActionListener(e -> m_clear());




           m_computebtn.addActionListener(new ActionListener() {
               @Override
               public void actionPerformed(ActionEvent e) {
                   String prod_id = prod_id_txt.getText().trim();
                   boolean isDuplicate = false;




                   if (prod_id.isEmpty()) {
                       JOptionPane.showMessageDialog(null, "Enter Product ID.");
                       return;
                   }




                   int quantity, price;
                   try {
                       quantity = Integer.parseInt(quan_txt.getText().trim());
                       price = Integer.parseInt(price_txt.getText().trim());
                   } catch (NumberFormatException ex) {
                       JOptionPane.showMessageDialog(null, "Enter valid numbers for quantity and price.");
                       return;
                   }




                   if (quantity < 0) {
                       JOptionPane.showMessageDialog(null, "Quantity must be 0 or greater.");
                       return;
                   }




                   if (price <= 0) {
                       JOptionPane.showMessageDialog(null, "Price must be greater than 0.");
                       return;
                   }




                   File file = new File("inventory.txt");
                   if (file.exists()) {
                       try (BufferedReader br = new BufferedReader(new FileReader("inventory.txt"))) {
                           String line;
                           while ((line = br.readLine()) != null) {
                               if (line.startsWith("Product ID: " + prod_id + ",")) {
                                   isDuplicate = true;
                                   break;
                               }
                           }
                       } catch (IOException ex) {
                           JOptionPane.showMessageDialog(null, "Error reading file: " + ex.getMessage());
                           return;
                       }
                   }




                   if (isDuplicate) {
                       JOptionPane.showMessageDialog(null, "Duplicate Product ID found.");
                   } else {
                       int total_value = quantity * price;
                       total_value_txt.setText(String.valueOf(total_value));
                   }
               }
           });




           m_loadRecords();
           setVisible(true);
       }
   }




   // +++ SALES +++
   public class sales extends JFrame{




       JFrame previousFrame;




       public sales(JFrame prev){
           this.previousFrame = prev;




           setTitle("Process Sales");
           setSize(1000, 600);
           setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
           setLocationRelativeTo(null);
           setLayout(new BorderLayout());




           s_prod_id_txt = new JTextField(20);




           s_prod_name_txt = new JTextField(20);
           s_prod_name_txt.setEditable(false);




           s_avail_quan_txt = new JTextField(20);
           s_avail_quan_txt.setEditable(false);




           s_quan_sold_txt = new JTextField(20);
           s_price_txt = new JTextField(20);
           s_price_txt.setEditable(false);




           s_total_sale_txt = new JTextField(20);
           s_total_sale_txt.setEditable(false);




           s_btnLoad = new JButton("Load");
           s_btnCompute = new JButton("Compute");
           s_btnSave = new JButton("Save Sale");
           s_clear = new JButton("Clear");
           s_btntotalSales = new JButton("Total Sales");




           JPanel titlePanel = new JPanel(new BorderLayout());
           titlePanel.setBackground(new Color(162, 207, 218));
           JLabel title = new JLabel("Process Sales", SwingConstants.CENTER);
           title.setFont(new Font("Arial", Font.BOLD, 30));
           title.setForeground(Color.WHITE);
           title.setBorder(BorderFactory.createEmptyBorder(15, 0, 15, 0));
           titlePanel.add(title, BorderLayout.CENTER);




           JPanel fieldsPanel = new JPanel(new GridLayout(6, 2, 10, 10));
           fieldsPanel.setBackground(new Color(162, 207, 218));
           fieldsPanel.setBorder(BorderFactory.createEmptyBorder(10, 40, 10, 40));




           fieldsPanel.add(new JLabel("Product ID:"));
           s_prod_id_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(s_prod_id_txt);




           fieldsPanel.add(new JLabel("Product Name:"));
           s_prod_name_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(s_prod_name_txt);




           fieldsPanel.add(new JLabel("Available Quantity:"));
           s_avail_quan_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(s_avail_quan_txt);




           fieldsPanel.add(new JLabel("Quantity Sold:"));
           s_quan_sold_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(s_quan_sold_txt);




           fieldsPanel.add(new JLabel("Price:"));
           s_price_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(s_price_txt);




           fieldsPanel.add(new JLabel("Total Sale:"));
           s_total_sale_txt.setFont(new Font("Arial", Font.PLAIN, 20));
           fieldsPanel.add(s_total_sale_txt);




           JPanel btnPanel = new JPanel(new GridLayout(1, 5, 10, 10));
           btnPanel.setBackground(new Color(162, 207, 218));
           btnPanel.setBorder(BorderFactory.createEmptyBorder(10, 40, 20, 40));




           btnPanel.add(s_btnLoad);
           s_btnLoad.setFont(new Font("Arial", Font.BOLD, 20));
           s_btnLoad.setBackground(new Color(163, 189, 255));
           s_btnLoad.setForeground(Color.WHITE);




           btnPanel.add(s_btnCompute);
           s_btnCompute.setFont(new Font("Arial", Font.BOLD, 20));
           s_btnCompute.setBackground(new Color(116, 222, 114));
           s_btnCompute.setForeground(Color.WHITE);




           btnPanel.add(s_btnSave);
           s_btnSave.setFont(new Font("Arial", Font.BOLD, 20));
           s_btnSave.setBackground(new Color(255, 163, 163));
           s_btnSave.setForeground(Color.WHITE);




           btnPanel.add(s_clear);
           s_clear.setFont(new Font("Arial", Font.BOLD, 20));
           s_clear.setBackground(new Color(126, 126, 126));
           s_clear.setForeground(new Color(253, 253, 253));




           btnPanel.add(s_btntotalSales);
           s_btntotalSales.setFont(new Font("Arial", Font.BOLD, 20));
           s_btntotalSales.setBackground(new Color(255, 215, 122));
           s_btntotalSales.setForeground(new Color(253, 253, 253));




           add(titlePanel, BorderLayout.NORTH);
           add(fieldsPanel, BorderLayout.CENTER);
           add(btnPanel, BorderLayout.SOUTH);




           s_btnLoad.addActionListener(e -> s_load());
           s_btnCompute.addActionListener(e -> s_compute());
           s_btnSave.addActionListener(e -> s_save());
           s_btntotalSales.addActionListener(e -> s_total_sales());




           s_clear.addActionListener(new ActionListener() {
               @Override
               public void actionPerformed(ActionEvent e) {
                   s_prod_id_txt.setText("");
                   s_prod_name_txt.setText("");
                   s_avail_quan_txt.setText("");
                   s_quan_sold_txt.setText("");
                   s_price_txt.setText("");
                   s_total_sale_txt.setText("");
               }
           });




           setVisible(true);
       }
   }




// ++++++++++++++++++ FOR MANAGE METHODS ++++++++++++++++++




   private void m_save(){
       String prod_ID = prod_id_txt.getText();
       String prod_name = prod_name_txt.getText();
       String quantity = quan_txt.getText();
       String price = price_txt.getText();
       String total_value = total_value_txt.getText();




       if (prod_ID.isEmpty() || prod_name.isEmpty() || quantity.isEmpty() || price.isEmpty() || total_value.isEmpty()) {
           JOptionPane.showMessageDialog(this, "Please fill all fields");
           return;
       }




       File file = new File("inventory.txt");
       if (file.exists()) {
           try (BufferedReader br = new BufferedReader(new FileReader("inventory.txt"))) {
               String line;
               while ((line = br.readLine()) != null) {
                   if (line.startsWith("Product ID: " + prod_ID + ",")) {
                       JOptionPane.showMessageDialog(this, "Duplicate Product ID! Record not saved.");
                       return;
                   }
               }
           } catch (IOException ex) {
               JOptionPane.showMessageDialog(this, "Error checking file: " + ex.getMessage());
               return;
           }
       }




       try (BufferedWriter bw = new BufferedWriter(new FileWriter("inventory.txt", true))) {
           bw.write("Product ID: " + prod_ID + ", Product name: " + prod_name + ", Quantity: " + quantity + ", Price: " + price + ", Total value: " + total_value);
           bw.newLine();
           JOptionPane.showMessageDialog(this, "Record saved!");
           prod_id_txt.setText("");
           prod_name_txt.setText("");
           quan_txt.setText("");
           price_txt.setText("");
           total_value_txt.setText("");
       } catch (IOException ex) {
           JOptionPane.showMessageDialog(this, "Error saving file");
       }




       m_loadRecords();
   }




   private void m_delete(){
       String prod_ID = prod_id_txt.getText();




       if (prod_ID.isEmpty()) {
           JOptionPane.showMessageDialog(this, "Please fill in product ID");
           return;
       }




       int confirm = JOptionPane.showConfirmDialog(this,
               "Are you sure you want to delete Product ID: " + prod_ID + "?",
               "Confirm Delete",
               JOptionPane.YES_NO_OPTION);
       if (confirm != JOptionPane.YES_OPTION) return;




       File file = new File("inventory.txt");
       File tempFile = new File("INVENTORY_temp.txt");
       boolean found = false;




       try(BufferedReader br = new BufferedReader(new FileReader(file));
           BufferedWriter bw = new BufferedWriter(new FileWriter(tempFile))){




           String line;
           while((line = br.readLine()) != null){
               if (!line.startsWith("Product ID: " + prod_ID + ", Product name: ")){
                   bw.write(line);
                   bw.newLine();
               } else {
                   found = true;
               }
           }
       } catch (IOException ex){
           JOptionPane.showMessageDialog(this, "Error deleting record");
       }




       if (found){
           file.delete();
           tempFile.renameTo(file);
           JOptionPane.showMessageDialog(this, "Record deleted!");
           prod_id_txt.setText("");
           prod_name_txt.setText("");
           quan_txt.setText("");
           price_txt.setText("");
           total_value_txt.setText("");
           m_loadRecords();
       } else {
           tempFile.delete();
           JOptionPane.showMessageDialog(this, "Product ID not found.");
       }
   }




   private void m_search(){
       String prod_id = prod_id_txt.getText();




       if(prod_id.isEmpty()){
           JOptionPane.showMessageDialog(this, "Enter Product ID to search");
           return;
       }




       try(BufferedReader br = new BufferedReader(new FileReader("inventory.txt"))){
           String line;
           boolean found = false;




           while((line = br.readLine()) != null){
               if (line.startsWith("Product ID: " + prod_id + ", Product name: ")){
                   String prod_name  = line.split(", ")[1].replace("Product name: ", "").trim();
                   prod_name_txt.setText(prod_name);




                   String quantity = line.split(", ")[2].replace("Quantity: ", "").trim();
                   quan_txt.setText(quantity);




                   String price = line.split(", ")[3].replace("Price: ", "").trim();
                   price_txt.setText(price);




                   String total_val = line.split(", ")[4].replace("Total value: ", "").trim();
                   total_value_txt.setText(total_val);




                   found = true;
                   break;
               }
           }




           if(!found){
               JOptionPane.showMessageDialog(this, "ID not found");
               prod_id_txt.setText("");
               prod_name_txt.setText("");
               quan_txt.setText("");
               price_txt.setText("");
               total_value_txt.setText("");
           } else {
               JOptionPane.showMessageDialog(this, "Record found!");
           }




       } catch(IOException ex){
           JOptionPane.showMessageDialog(this, "Error reading file: " + ex.getMessage());
       }
   }




   private void m_clear(){
       prod_id_txt.setText("");
       prod_name_txt.setText("");
       quan_txt.setText("");
       price_txt.setText("");
       total_value_txt.setText("");
   }








// ++++++++++++++++++ FOR SALES METHODS ++++++++++++++++++




   private void s_load() {
       String prod_id = s_prod_id_txt.getText().trim();




       if (prod_id.isEmpty()) {
           JOptionPane.showMessageDialog(this, "Enter a Product ID to load.");
           return;
       }




       File file = new File("inventory.txt");
       if (!file.exists()) {
           JOptionPane.showMessageDialog(this, "Inventory file not found.");
           return;
       }




       try (BufferedReader br = new BufferedReader(new FileReader("inventory.txt"))) {
           String line;
           while ((line = br.readLine()) != null) {
               if (line.startsWith("Product ID: " + prod_id + ",")) {
                   String prod_name = line.substring(line.indexOf("Product name: ") + 14, line.indexOf(", Quantity:")).trim();
                   String quantity = line.substring(line.indexOf("Quantity: ") + 10, line.indexOf(", Price:")).trim();
                   String price = line.substring(line.indexOf("Price: ") + 7, line.indexOf(", Total value:")).trim();




                   if(Integer.parseInt(quantity) < 5){
                       s_avail_quan_txt.setBackground(new Color(248, 47, 47));
                   } else {
                       s_avail_quan_txt.setBackground(Color.WHITE);
                   }




                   s_prod_name_txt.setText(prod_name);
                   s_avail_quan_txt.setText(quantity);
                   s_price_txt.setText(price);
                   return;
               }
           }
           JOptionPane.showMessageDialog(this, "Product ID not found.");
       } catch (IOException ex) {
           JOptionPane.showMessageDialog(this, "Error reading inventory: " + ex.getMessage());
       }
   }




   private void s_compute() {
       try {
           int availQty = Integer.parseInt(s_avail_quan_txt.getText().trim());
           int qtySold = Integer.parseInt(s_quan_sold_txt.getText().trim());
           int price = Integer.parseInt(s_price_txt.getText().trim());




           if (availQty <= 0) {
               JOptionPane.showMessageDialog(this, "No stock available.");
               return;
           }




           if (qtySold <= 0) {
               JOptionPane.showMessageDialog(this, "Quantity sold must be positive.");
               return;
           }




           if (qtySold > availQty) {
               JOptionPane.showMessageDialog(this, "Insufficient Stock!");
               return;
           }




           int totalSale = qtySold * price;
           s_total_sale_txt.setText(String.valueOf(totalSale));




       } catch (NumberFormatException ex) {
           JOptionPane.showMessageDialog(this, "Please load a product and enter a valid quantity.");
       }
   }




   private void s_save() {
       String prod_id = s_prod_id_txt.getText().trim();
       String prod_name = s_prod_name_txt.getText().trim();
       String qtySold = s_quan_sold_txt.getText().trim();
       String price = s_price_txt.getText().trim();
       String totalSale = s_total_sale_txt.getText().trim();




       if (prod_id.isEmpty() || prod_name.isEmpty() || qtySold.isEmpty() || totalSale.isEmpty()) {
           JOptionPane.showMessageDialog(this, "Please load a product and compute first.");
           return;
       }




       try (BufferedWriter bw = new BufferedWriter(new FileWriter("sales.txt", true))) {
           bw.write("Product ID: " + prod_id + ", Product name: " + prod_name + ", Quantity sold: " + qtySold + ", Price: " + price + ", Total sales: " + totalSale);
           bw.newLine();
       } catch (IOException ex) {
           JOptionPane.showMessageDialog(this, "Error saving sale: " + ex.getMessage());
           return;
       }




       s_deductFromInventory(prod_id, Integer.parseInt(qtySold));
   }




   private void s_deductFromInventory(String prod_id, int qtySold) {
       File file = new File("inventory.txt");
       ArrayList<String> updated = new ArrayList<>();
       boolean found = false;




       try (BufferedReader br = new BufferedReader(new FileReader(file))) {
           String line;
           while ((line = br.readLine()) != null) {
               if (line.startsWith("Product ID: " + prod_id + ",")) {
                   int currentQty = Integer.parseInt(
                           line.substring(line.indexOf("Quantity: ") + 10, line.indexOf(", Price:")).trim()
                   );
                   int newQty = currentQty - qtySold;
                   line = line.replace("Quantity: " + currentQty, "Quantity: " + newQty);
                   s_avail_quan_txt.setText(String.valueOf(newQty));
                   found = true;
               }
               updated.add(line);
           }
       } catch (IOException ex) {
           JOptionPane.showMessageDialog(this, "Error reading inventory: " + ex.getMessage());
           return;
       }




       if (found) {
           try (BufferedWriter bw = new BufferedWriter(new FileWriter(file))) {
               for (String record : updated) {
                   bw.write(record);
                   bw.newLine();
               }
               JOptionPane.showMessageDialog(this, "Sale saved and inventory updated!");
               s_quan_sold_txt.setText("");
               s_total_sale_txt.setText("");
           } catch (IOException ex) {
               JOptionPane.showMessageDialog(this, "Error updating inventory: " + ex.getMessage());
           }
       }
   }




   private void s_total_sales(){
       s_prod_id_txt.setText("");
       s_prod_name_txt.setText("");
       s_avail_quan_txt.setText("");
       s_quan_sold_txt.setText("");
       s_price_txt.setText("");
       s_total_sale_txt.setText("");
       s_avail_quan_txt.setEditable(false);
       s_avail_quan_txt.setBackground(Color.WHITE);
       s_prod_id_txt.setEditable(false);
       s_quan_sold_txt.setEditable(false);




       // FIX: reset counters before accumulating to avoid double-counting on repeated calls
       int runningQty = 0;
       int runningTotal = 0;




       try(BufferedReader br = new BufferedReader(new FileReader("sales.txt"))){
           String line;
           while((line = br.readLine()) != null) {
               String quantity = line.substring(line.indexOf("Quantity sold: ") + 15, line.indexOf(", Price: "));
               runningQty += Integer.parseInt(quantity);




               String total_val = line.substring(line.indexOf("Total sales: ") + 13);
               runningTotal += Integer.parseInt(total_val);
           }




           s_quan_sold_txt.setText(String.valueOf(runningQty));
           s_total_sale_txt.setText(String.valueOf(runningTotal));




       } catch(IOException ex){
           JOptionPane.showMessageDialog(this, "Error reading file: " + ex.getMessage());
       }
   }




   private void m_loadRecords() {
       inventory.clear();
       File file = new File("inventory.txt");
       if (!file.exists()) return;




       try (BufferedReader br = new BufferedReader(new FileReader("inventory.txt"))) {
           String line;
           while ((line = br.readLine()) != null) {
               if (!line.trim().isEmpty()) {
                   inventory.add(line);
               }
           }
       } catch (IOException ex) {
           JOptionPane.showMessageDialog(null, "Error loading records: " + ex.getMessage());
       }




       if (!inventory.isEmpty()) {
           currentIndex = 0;
           m_displayRecord(currentIndex);
       }
   }




   private void m_displayRecord(int index) {
       if (index >= 0 && index < inventory.size()) {
           String line = inventory.get(index);
           try {
               String prod_id = line.substring(line.indexOf("Product ID: ") + 12, line.indexOf(", Product name:")).trim();
               String prod_name = line.substring(line.indexOf("Product name: ") + 14, line.indexOf(", Quantity:")).trim();
               String quantity = line.substring(line.indexOf("Quantity: ") + 10, line.indexOf(", Price:")).trim();
               String price = line.substring(line.indexOf("Price: ") + 7, line.indexOf(", Total value:")).trim();
               String total_val = line.substring(line.indexOf("Total value: ") + 13).trim();




               prod_id_txt.setText(prod_id);
               prod_name_txt.setText(prod_name);
               quan_txt.setText(quantity);
               price_txt.setText(price);
               total_value_txt.setText(total_val);
           } catch (Exception ex) {
               JOptionPane.showMessageDialog(null, "Error displaying record: " + ex.getMessage());
           }
       }
       m_updateButtons();
   }




   private void m_updateButtons() {
       if (m_btnPrevious != null)
           m_btnPrevious.setEnabled(currentIndex > 0);
       if (m_btnNext != null)
           m_btnNext.setEnabled(currentIndex < inventory.size() - 1);
   }








   // ++++++++++++++++++ FOR CASHIER CONSTRUCTOR ++++++++++++++++++




   public class cashier extends JFrame {




       public cashier() {
           setTitle("Simple Cashiering System");
           setSize(900, 500);
           setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
           setLocationRelativeTo(null);




           JPanel panel = new JPanel();
           panel.setLayout(new BorderLayout());
           add(panel);




           JPanel inputPanel = new JPanel(new GridLayout(2, 5, 10, 10));
           // Initialize fields
           txtID = new JTextField();
           txtItem = new JTextField();
           txtItem.setEditable(false);
           txtPrice = new JTextField();
           txtPrice.setEditable(false);
           txtQty = new JTextField();




           JButton btnAdd = new JButton("Add Item");
           // Add labels to fields and add to panel
           inputPanel.add(new JLabel("Item ID:"));
           inputPanel.add(new JLabel("Item Name:"));
           inputPanel.add(new JLabel("Price:"));
           inputPanel.add(new JLabel("Quantity:"));
           inputPanel.add(new JLabel(""));
           inputPanel.add(txtID);
           inputPanel.add(txtItem);
           inputPanel.add(txtPrice);
           inputPanel.add(txtQty);
           inputPanel.add(btnAdd);
           panel.add(inputPanel, BorderLayout.NORTH);




           model = new DefaultTableModel();
           model.setColumnIdentifiers(new String[]{"Item", "Price", "Quantity", "Subtotal"});
           table = new JTable(model);
           panel.add(new JScrollPane(table), BorderLayout.CENTER);




           JPanel bottomPanel = new JPanel(new GridLayout(3, 2, 10, 10));
           lblTotal = new JLabel("Total: 0.00");
           txtCash = new JTextField();
           lblChange = new JLabel("Change: 0.00");




           JButton btnPay = new JButton("Pay");
           bottomPanel.add(lblTotal);
           bottomPanel.add(new JLabel(""));
           bottomPanel.add(new JLabel("Cash:"));
           bottomPanel.add(txtCash);
           bottomPanel.add(lblChange);
           bottomPanel.add(btnPay);
           panel.add(bottomPanel, BorderLayout.SOUTH);




           // FIX: use ActionListener (Enter key) instead of KeyListener
           txtID.addActionListener(e -> c_search());








           // Add Item Button Action
           btnAdd.addActionListener(e -> {
               try {
                   String ID = txtID.getText();
                   String item = txtItem.getText();
                   double price = Double.parseDouble(txtPrice.getText());
                   int qty = Integer.parseInt(txtQty.getText());
                   double subtotal = price * qty;




                   if (quantity_from_inventory < qty){
                       JOptionPane.showMessageDialog(this, "Insufficient stock available. " +
                               "There is only " + quantity_from_inventory + " left for this item.");
                   } else {
                       total += subtotal;
                       model.addRow(new Object[]{item, price, qty, subtotal});
                       lblTotal.setText("Total: " + String.format("%.2f", total));
                       c_deductFromInventory(ID, qty);
                   }




               } catch (Exception ex) {
                   JOptionPane.showMessageDialog(this, "Please enter valid values!");
               }
           });




           // Pay Button Action
           btnPay.addActionListener(e -> {
               try {
                   double cash = Double.parseDouble(txtCash.getText());
                   if (cash < total) {
                       JOptionPane.showMessageDialog(this, "Insufficient cash!");
                   } else {
                       double change = cash - total;
                       lblChange.setText("Change: " + String.format("%.2f", change));
                       c_save_transactions();




                       // FIX: reset total and clear table after successful payment
                       total = 0.0;
                       model.setRowCount(0);
                       txtID.setText("");
                       txtItem.setText("");
                       txtPrice.setText("");
                       txtQty.setText("");
                       txtCash.setText("");
                       lblTotal.setText("Total: 0.00");
                       lblChange.setText("Change: 0.00");
                   }
               } catch (Exception ex) {
                   JOptionPane.showMessageDialog(this, "Enter valid cash amount!");
               }
           });
       }
   }




   // == search for cashier ==
   private void c_search(){
       String ID = txtID.getText();




       // FIX: return early if ID is empty so we don't try to read the file
       if(ID.isEmpty()){
           JOptionPane.showMessageDialog(null, "Enter Product ID to continue");
           return;
       }




       try(BufferedReader br = new BufferedReader(new FileReader("inventory.txt"))){
           String line;
           boolean found = false;
           String quantity = "";




           while((line = br.readLine()) != null){
               if (line.contains("Product ID: " + ID + ",")){
                   String prod_name = line.substring(line.indexOf("Product name: ") + 14, line.indexOf(", Quantity:")).trim();
                   txtItem.setText(prod_name);




                   quantity = line.substring(line.indexOf("Quantity: ") + 10, line.indexOf(", Price:")).trim();




                   String price = line.substring(line.indexOf("Price: ") + 7, line.indexOf(", Total value:")).trim();
                   txtPrice.setText(price);




                   found = true;
                   break;
               }
           }




           if(!found){
               JOptionPane.showMessageDialog(null, "ID not found");
               quantity_from_inventory = 0;
               txtItem.setText("");
               txtQty.setText("");
               txtPrice.setText("");
           } else {
               JOptionPane.showMessageDialog(null, "Record found!");
               quantity_from_inventory = Integer.parseInt(quantity);
           }




       } catch(IOException ex){
           JOptionPane.showMessageDialog(null, "Error reading file: " + ex.getMessage());
       }
   }




   private void c_deductFromInventory(String prod_id, int qtySold) {
       File file = new File("inventory.txt");
       ArrayList<String> updated = new ArrayList<>();
       boolean found = false;




       try (BufferedReader br = new BufferedReader(new FileReader(file))) {
           String line;
           while ((line = br.readLine()) != null) {
               if (line.startsWith("Product ID: " + prod_id + ",")) {
                   int currentQty = Integer.parseInt(
                           line.substring(line.indexOf("Quantity: ") + 10, line.indexOf(", Price:")).trim()
                   );
                   int newQty = currentQty - qtySold;
                   line = line.replace("Quantity: " + currentQty, "Quantity: " + newQty);
                   found = true;
               }
               updated.add(line);
           }
       } catch (IOException ex) {
           JOptionPane.showMessageDialog(this, "Error reading inventory from cashier: " + ex.getMessage());
           return;
       }




       if (found) {
           try (BufferedWriter bw = new BufferedWriter(new FileWriter(file))) {
               for (String record : updated) {
                   bw.write(record);
                   bw.newLine();
               }
               JOptionPane.showMessageDialog(this, "Inventory updated!");
           } catch (IOException ex) {
               JOptionPane.showMessageDialog(this, "Error updating inventory from cashier: " + ex.getMessage());
           }
       }
   }




   private void c_save_transactions(){
       String ID = txtID.getText();
       String item_name = txtItem.getText();
       String quantity = txtQty.getText();
       String price = txtPrice.getText();
       String total_value = lblTotal.getText();
       String cash = txtCash.getText();
       String change = lblChange.getText();




       if (ID.isEmpty() || item_name.isEmpty() || quantity.isEmpty() || price.isEmpty()
               || total_value.isEmpty() || cash.isEmpty() || change.isEmpty()) {
           JOptionPane.showMessageDialog(this, "Please fill all fields");
           return;
       }


       try (BufferedWriter bw = new BufferedWriter(new FileWriter("transactions.txt", true))) {
           bw.write("Product ID: " + ID + ", Product name: " + item_name + ", Quantity: " + quantity
                   + ", Price: " + price + ", Total value: " + total_value
                   + ", Cash: " + cash + ", Change: " + change);
           bw.newLine();
           JOptionPane.showMessageDialog(this, "Transaction saved!");
       } catch (IOException ex) {
           JOptionPane.showMessageDialog(this, "Error saving file");
       }
   }








   public static void main(String[] args) {
       SwingUtilities.invokeLater(() -> {
           new cashier_clean().setVisible(true);
       });
   }
}




