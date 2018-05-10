# Final-year-project
This is the code for my final year project.

AdminDatabaseScreen

package smart.budgeting.app;

import DatabaseConnection.MySqlConnect;
import com.opencsv.CSVReader;
import java.io.*;
import java.sql.*;
import java.util.*;
import javax.swing.JFileChooser;
import javax.swing.JOptionPane;
import javax.swing.RowFilter;
import javax.swing.filechooser.FileNameExtensionFilter;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.TableModel;
import javax.swing.table.TableRowSorter;
import net.proteanit.sql.DbUtils;


public class AdminDatabaseScreen extends javax.swing.JFrame {

    Connection connection = MySqlConnect.ConnectDB();
    PreparedStatement pstmt = null;
    ResultSet rs = null;

    JFileChooser csvChooser = new JFileChooser();
    FileNameExtensionFilter filter = new FileNameExtensionFilter(".csv", "csv");
    File file;
    
    public Vector<Vector<String>> data;
    public Vector<String> dataHeader;
    
    public AdminDatabaseScreen() {
        initComponents();
        defaultLoadTable();
    }

    //a table obtained locally will be displayed on the table to demonstrate the functionalities in this screen.
    private void defaultLoadTable() {
        try {
            String defaultTable = "SELECT providerName, subject, grads, lowerAnnEarn, medianAnnEarn, upperAnnEarn FROM sfr18_17data";
            pstmt = connection.prepareStatement(defaultTable);
            rs = pstmt.executeQuery(defaultTable);
        
            TableModel model = DbUtils.resultSetToTableModel(rs);
            databaseTable.setModel(model);
            
        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, "Cannot retrieve table." + e);
                    
        }
    }
    
    //the administrator can filter a table by typing in specific letters.
    public void filterTable(String getQuery) {
      TableRowSorter<TableModel> tableSorter = new TableRowSorter<>(databaseTable.getModel());
      
      databaseTable.setRowSorter(tableSorter);
      tableSorter.setRowFilter(RowFilter.regexFilter(getQuery));
    }
    

    public String uploadData() {
        csvChooser.setFileFilter(filter);
        
        int getVal = csvChooser.showOpenDialog(this);
        if (getVal == JFileChooser.APPROVE_OPTION) {
            file = csvChooser.getSelectedFile();
            if (file.length() >= 1024) {
                JOptionPane.showMessageDialog(this, "This file is too large. All files must be less than 1024 MB.");
            }
            if (file != null) {
                return file.getAbsolutePath();
            }
            else {
                JOptionPane.showMessageDialog(this, "Please select a file");
            }
        }
        return null;
    }
    
    public void readFromCSVFIle(String fileName) {       
        JFileChooser CSVChooser = new JFileChooser(file);
        int returnVal = CSVChooser.showOpenDialog(this);
        if (returnVal == JFileChooser.APPROVE_OPTION) {
            file = CSVChooser.getSelectedFile();
            try {
                CSVReader reader = new CSVReader(new FileReader(file.getAbsolutePath()));
                List entries = reader.readAll();
                Object[] rows = entries.toArray();
                
            } catch (IOException e) {
                JOptionPane.showMessageDialog(this, "Cannot retrieve file: " + file.getAbsolutePath() + "." + e.getMessage());
            }
        }
    }

    
    public void updateTable() {
        try {
            if (file.getAbsolutePath().contains("student_infosave")) {
                String studentSQL = "SELECT * FROM student_infosave";
                pstmt = connection.prepareStatement(studentSQL);
                pstmt.executeUpdate();
            }
            else if (file.getAbsolutePath().contains("admin_infosave")) {
                String adminSQL = "SELECT * FROM admin_infosave";
                pstmt = connection.prepareStatement(adminSQL);
                pstmt.executeUpdate();
            }
        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, e);
        }
    }
    
    public void readCSVFile() {
        FileNameExtensionFilter filterFile = new FileNameExtensionFilter("Database file", ".csv", "csv");
        final JFileChooser getFile = new JFileChooser();
        getFile.setApproveButtonText("Import database");
        getFile.setFileFilter(filterFile);
        
        List<String[]> element = new ArrayList<>();
        DefaultTableModel tableModel = (DefaultTableModel) databaseTable.getModel();
        
        int actionDialog = getFile.showOpenDialog(this);
        if (actionDialog != JFileChooser.APPROVE_OPTION) {
            return;
        }
        File CSVfile = getFile.getSelectedFile();
        if(!CSVfile.getName().endsWith(".csv")) {
            CSVfile = new File(CSVfile.getAbsolutePath() + ".csv");
        }
        
        BufferedReader readFile = null;
        try {
            readFile = new BufferedReader(new FileReader(file));
            for (int i = 0; i < databaseTable.getRowCount(); i++) {
                String line;
                while ((line = readFile.readLine()) != null) {
                    tableModel.addRow(line.split("\t"));
                }
            }
            readFile.close();
        } catch(Exception e) {
            JOptionPane.showMessageDialog(null, e);
        }        
    }

  
    /**
     * This method is called from within the constructor to initialize the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        jScrollPane1 = new javax.swing.JScrollPane();
        databaseTable = new javax.swing.JTable();
        uploadBtn = new javax.swing.JButton();
        goBackBtn = new javax.swing.JButton();
        clearBtn = new javax.swing.JButton();
        saveBtn = new javax.swing.JButton();
        saveNewBtn = new javax.swing.JButton();
        filterTextLabel = new javax.swing.JLabel();
        filterSearchTextBox = new javax.swing.JTextField();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        databaseTable.setModel(new javax.swing.table.DefaultTableModel(
            new Object [][] {
                {null, null, null, null, null, null},
                {null, null, null, null, null, null},
                {null, null, null, null, null, null},
                {null, null, null, null, null, null}
            },
            new String [] {
                "Title 1", "Title 2", "Title 3", "Title 4", "Title 5", "Title 6"
            }
        ));
        jScrollPane1.setViewportView(databaseTable);

        uploadBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        uploadBtn.setText("Upload Database");
        uploadBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                uploadBtnActionPerformed(evt);
            }
        });

        goBackBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        goBackBtn.setText("Go Back");
        goBackBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                goBackBtnActionPerformed(evt);
            }
        });

        clearBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        clearBtn.setText("Clear Database");
        clearBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                clearBtnActionPerformed(evt);
            }
        });

        saveBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        saveBtn.setText("Save Database");
        saveBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                saveBtnActionPerformed(evt);
            }
        });

        saveNewBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        saveNewBtn.setText("Save New Database");
        saveNewBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                saveNewBtnActionPerformed(evt);
            }
        });

        filterTextLabel.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        filterTextLabel.setText("Filter search:");

        filterSearchTextBox.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        filterSearchTextBox.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                filterSearchTextBoxActionPerformed(evt);
            }
        });

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGap(30, 30, 30)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING, false)
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(uploadBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 133, javax.swing.GroupLayout.PREFERRED_SIZE)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                        .addComponent(clearBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 133, javax.swing.GroupLayout.PREFERRED_SIZE)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                        .addComponent(saveBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 133, javax.swing.GroupLayout.PREFERRED_SIZE)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                        .addComponent(saveNewBtn)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                        .addComponent(goBackBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 133, javax.swing.GroupLayout.PREFERRED_SIZE))
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(filterTextLabel)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                        .addComponent(filterSearchTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, 179, javax.swing.GroupLayout.PREFERRED_SIZE))
                    .addComponent(jScrollPane1))
                .addContainerGap(21, Short.MAX_VALUE))
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGap(50, 50, 50)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(filterTextLabel)
                    .addComponent(filterSearchTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.UNRELATED)
                .addComponent(jScrollPane1, javax.swing.GroupLayout.PREFERRED_SIZE, 223, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, 8, Short.MAX_VALUE)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(uploadBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 42, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(clearBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 42, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(saveBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 42, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(saveNewBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 42, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(goBackBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 42, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addContainerGap())
        );

        pack();
    }// </editor-fold>                        

    private void clearBtnActionPerformed(java.awt.event.ActionEvent evt) {                                         
        DefaultTableModel tableModel = (DefaultTableModel) (databaseTable.getModel());
        tableModel.setRowCount(0);
    }                                        

    private void uploadBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.readCSVFile();
    }                                         

    private void goBackBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.setVisible(false);
        new AdminHomeScreen().setVisible(true);
    }                                         

    private void saveBtnActionPerformed(java.awt.event.ActionEvent evt) {                                        
        JOptionPane.showConfirmDialog(null, "Save current or new file?");
    }                                       

    private void saveNewBtnActionPerformed(java.awt.event.ActionEvent evt) {                                           
        //a new table will be saved locally as a CSV file with values modified by the administrator.
        FileNameExtensionFilter filter = new FileNameExtensionFilter ("CSV file", ".csv", "csv");
        final JFileChooser saveCSVFile = new JFileChooser();
        saveCSVFile.setApproveButtonText("Save New Database");
        saveCSVFile.setFileFilter(filter);
        
        int actionDialog = saveCSVFile.showOpenDialog(this);
        if (actionDialog != JFileChooser.APPROVE_OPTION) {
            return;
        }
        
        File CSVFile = saveCSVFile.getSelectedFile();
        if (!CSVFile.getName().endsWith(".csv")) {
            CSVFile = new File(CSVFile.getAbsolutePath() + ".csv");
        }
        
        BufferedWriter writeFile = null;
        try {
            writeFile = new BufferedWriter(new FileWriter(CSVFile));
            for (int i = 0; i < databaseTable.getRowCount(); i++) {
                writeFile.newLine();
                for (int j = 0; j < databaseTable.getColumnCount(); j++) {
                    writeFile.write((String)(databaseTable.getValueAt(i, j)));
                    writeFile.write("\t");
                }
            }
            writeFile.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }                                          

    private void filterSearchTextBoxActionPerformed(java.awt.event.ActionEvent evt) {                                                    
        String getQuery = filterSearchTextBox.getText();
        filterTable(getQuery);
    }                                                   

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(AdminDatabaseScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(AdminDatabaseScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(AdminDatabaseScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(AdminDatabaseScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new AdminDatabaseScreen().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify                     
    private javax.swing.JButton clearBtn;
    private javax.swing.JTable databaseTable;
    private javax.swing.JTextField filterSearchTextBox;
    private javax.swing.JLabel filterTextLabel;
    private javax.swing.JButton goBackBtn;
    private javax.swing.JScrollPane jScrollPane1;
    private javax.swing.JButton saveBtn;
    private javax.swing.JButton saveNewBtn;
    private javax.swing.JButton uploadBtn;
    // End of variables declaration                   
}

AdminHomeScreen

package smart.budgeting.app;


public class AdminHomeScreen extends javax.swing.JFrame {

    /**
     * Creates new form AdminHomeScreen
     */
    public AdminHomeScreen() {
        initComponents();
    }

    /**
     * This method is called from within the constructor to initialize the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        databaseBtn = new javax.swing.JButton();
        recordsBtn = new javax.swing.JButton();
        reportBtn = new javax.swing.JButton();
        logOutBtn = new javax.swing.JButton();
        jLabel1 = new javax.swing.JLabel();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);
        setBackground(new java.awt.Color(255, 255, 255));
        setMinimumSize(new java.awt.Dimension(840, 450));
        setSize(new java.awt.Dimension(760, 360));
        getContentPane().setLayout(new org.netbeans.lib.awtextra.AbsoluteLayout());

        databaseBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        databaseBtn.setText("Manage Databases");
        databaseBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                databaseBtnActionPerformed(evt);
            }
        });
        getContentPane().add(databaseBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(60, 170, 193, 166));

        recordsBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        recordsBtn.setText("Manage Student Records");
        recordsBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                recordsBtnActionPerformed(evt);
            }
        });
        getContentPane().add(recordsBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(320, 170, 193, 166));

        reportBtn.setBackground(new java.awt.Color(255, 255, 255));
        reportBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        reportBtn.setText("Review Generated Reports");
        reportBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                reportBtnActionPerformed(evt);
            }
        });
        getContentPane().add(reportBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(580, 170, 193, 166));

        logOutBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        logOutBtn.setText("Log Out");
        logOutBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                logOutBtnActionPerformed(evt);
            }
        });
        getContentPane().add(logOutBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(690, 40, -1, -1));

        jLabel1.setIcon(new javax.swing.ImageIcon(getClass().getResource("/smart/budgeting/app/Spray-pattern-image.png"))); // NOI18N
        getContentPane().add(jLabel1, new org.netbeans.lib.awtextra.AbsoluteConstraints(10, 0, -1, -1));

        pack();
    }// </editor-fold>                        

    private void databaseBtnActionPerformed(java.awt.event.ActionEvent evt) {                                            
        this.setVisible(false);
        new AdminDatabaseScreen().setVisible(true);
    }                                           

    private void recordsBtnActionPerformed(java.awt.event.ActionEvent evt) {                                           
        this.setVisible(false);
        new AdminRecordsScreen().setVisible(true);
    }                                          

    private void reportBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.setVisible(false);
        new AdminReportScreen().setVisible(true);
    }                                         

    private void logOutBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.setVisible(false);
        new WelcomeScreen().setVisible(true);
    }                                         

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(AdminHomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(AdminHomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(AdminHomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(AdminHomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new AdminHomeScreen().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify                     
    private javax.swing.JButton databaseBtn;
    private javax.swing.JLabel jLabel1;
    private javax.swing.JButton logOutBtn;
    private javax.swing.JButton recordsBtn;
    private javax.swing.JButton reportBtn;
    // End of variables declaration                   
}

AdminLoginScreen

package smart.budgeting.app;

import DatabaseConnection.MySqlConnect;
import java.sql.*;
import javax.swing.JOptionPane;

public class AdminRegStudentScreen extends javax.swing.JFrame {
    
    Connection connection = MySqlConnect.ConnectDB();
    PreparedStatement pstmt = null;
    ResultSet rs = null;
    
    public AdminRegStudentScreen() {
        initComponents();
    }
    
    public void regStudent() {
        try {
        String regUser = "INSERT INTO student_infosave (First_name, Last_name, University, Degree_type) VALUES (?,?,?,?)";
        pstmt = connection.prepareStatement(regUser);
        
        pstmt.setString(1, FNTextBox.getText());
        pstmt.setString(2, LNTextBox.getText());
        pstmt.setString(3, UniTextBox.getText());
        pstmt.setString(4, DegreeComboBox.getSelectedItem().toString());
        
        pstmt.executeUpdate();

        JOptionPane.showMessageDialog(null, "You have registered the user successfully.");
        this.setVisible(false);
        } catch (SQLException e) {
            JOptionPane.showMessageDialog(null, e);
        }
    }

    /**
     * This method is called from within the constructor to initialize the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        FNTextBox = new javax.swing.JTextField();
        FirstName = new javax.swing.JLabel();
        LastName = new javax.swing.JLabel();
        LNTextBox = new javax.swing.JTextField();
        University = new javax.swing.JLabel();
        DegreeComboBox = new javax.swing.JComboBox<>();
        registerBtn = new javax.swing.JButton();
        DegreeType = new javax.swing.JLabel();
        UniTextBox = new javax.swing.JTextField();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        FNTextBox.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        FNTextBox.setText("null");

        FirstName.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        FirstName.setText("First name");

        LastName.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        LastName.setText("Last name");

        LNTextBox.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        LNTextBox.setText("null");

        University.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        University.setText("University");

        DegreeComboBox.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        DegreeComboBox.setModel(new javax.swing.DefaultComboBoxModel<>(new String[] { "Please select", "Undergraduate", "Postgraduate" }));

        registerBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        registerBtn.setText("Register");
        registerBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                registerBtnActionPerformed(evt);
            }
        });

        DegreeType.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        DegreeType.setText("Degree type");

        UniTextBox.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        UniTextBox.setText("null");

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addContainerGap(javax.swing.GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE)
                .addComponent(registerBtn)
                .addGap(55, 55, 55))
            .addGroup(layout.createSequentialGroup()
                .addGap(63, 63, 63)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.TRAILING, false)
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(University)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, javax.swing.GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE)
                        .addComponent(UniTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, 192, javax.swing.GroupLayout.PREFERRED_SIZE))
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(FirstName)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, 29, Short.MAX_VALUE)
                        .addComponent(FNTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, 192, javax.swing.GroupLayout.PREFERRED_SIZE)))
                .addGap(35, 35, 35)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(LastName)
                        .addGap(18, 18, 18)
                        .addComponent(LNTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, 192, javax.swing.GroupLayout.PREFERRED_SIZE))
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(DegreeType)
                        .addGap(18, 18, 18)
                        .addComponent(DegreeComboBox, javax.swing.GroupLayout.PREFERRED_SIZE, 192, javax.swing.GroupLayout.PREFERRED_SIZE)))
                .addContainerGap(92, Short.MAX_VALUE))
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGap(51, 51, 51)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(FNTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, 34, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(FirstName)
                    .addComponent(LNTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, 34, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(LastName))
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addGroup(layout.createSequentialGroup()
                        .addGap(95, 95, 95)
                        .addComponent(registerBtn)
                        .addContainerGap())
                    .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                            .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                                .addComponent(DegreeComboBox, javax.swing.GroupLayout.PREFERRED_SIZE, 39, javax.swing.GroupLayout.PREFERRED_SIZE)
                                .addComponent(DegreeType))
                            .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                                .addComponent(University)
                                .addComponent(UniTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, 34, javax.swing.GroupLayout.PREFERRED_SIZE)))
                        .addGap(74, 74, 74))))
        );

        pack();
    }// </editor-fold>                        

    private void registerBtnActionPerformed(java.awt.event.ActionEvent evt) {                                            
        this.regStudent();
    }                                           

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(AdminRegStudentScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(AdminRegStudentScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(AdminRegStudentScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(AdminRegStudentScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new AdminRegStudentScreen().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify                     
    private javax.swing.JComboBox<String> DegreeComboBox;
    private javax.swing.JLabel DegreeType;
    private javax.swing.JTextField FNTextBox;
    private javax.swing.JLabel FirstName;
    private javax.swing.JTextField LNTextBox;
    private javax.swing.JLabel LastName;
    private javax.swing.JTextField UniTextBox;
    private javax.swing.JLabel University;
    private javax.swing.JButton registerBtn;
    // End of variables declaration                   
}


AdminRecordsScreen

package smart.budgeting.app;

import DatabaseConnection.DatabaseConnectivity;
import DatabaseConnection.MySqlConnect;
import User.AdminController;
import smart.budgeting.app.*;
import java.sql.*;
import java.util.Arrays;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
/**
 *
 * @author Ahmed A. Ali
 */
public class AdminRegisterScreen extends javax.swing.JFrame {

Connection connection = null;
PreparedStatement pstmt = null;
ResultSet rs = null;
AdminController controller;
    
    
    public AdminRegisterScreen() {
        initComponents();
    }

    /**
     * This method is called from within the constructor to initialize the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        AdFNTextBox = new javax.swing.JTextField();
        AdLNTextBox = new javax.swing.JTextField();
        AdEmailTextBox = new javax.swing.JTextField();
        Password = new javax.swing.JLabel();
        FirstName = new javax.swing.JLabel();
        LastName = new javax.swing.JLabel();
        Email = new javax.swing.JLabel();
        AdRegBtn = new javax.swing.JButton();
        AdPassTextBox = new javax.swing.JPasswordField();
        goBackBtn = new javax.swing.JButton();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        AdFNTextBox.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N

        AdLNTextBox.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N

        AdEmailTextBox.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        AdEmailTextBox.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                AdEmailTextBoxActionPerformed(evt);
            }
        });

        Password.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        Password.setText("Password");

        FirstName.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        FirstName.setText("First name");

        LastName.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        LastName.setText("Last name");

        Email.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        Email.setText("Corporate E-mail");

        AdRegBtn.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        AdRegBtn.setText("Register");
        AdRegBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                AdRegBtnActionPerformed(evt);
            }
        });

        AdPassTextBox.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N

        goBackBtn.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        goBackBtn.setText("Go Back");
        goBackBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                goBackBtnActionPerformed(evt);
            }
        });

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addContainerGap(82, Short.MAX_VALUE)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addComponent(FirstName)
                    .addComponent(Email))
                .addGap(42, 42, 42)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING, false)
                    .addComponent(AdFNTextBox)
                    .addComponent(AdEmailTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, 160, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addGap(62, 62, 62)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addComponent(Password)
                    .addComponent(LastName))
                .addGap(18, 18, 18)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(AdRegBtn)
                        .addGap(18, 18, 18)
                        .addComponent(goBackBtn))
                    .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING, false)
                        .addComponent(AdLNTextBox)
                        .addComponent(AdPassTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, 161, javax.swing.GroupLayout.PREFERRED_SIZE)))
                .addGap(61, 61, 61))
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addContainerGap(119, Short.MAX_VALUE)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                            .addComponent(AdLNTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addComponent(LastName))
                        .addGap(33, 33, 33)
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                            .addComponent(Password)
                            .addComponent(AdPassTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)))
                    .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                            .addComponent(FirstName)
                            .addComponent(AdFNTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                        .addGap(30, 30, 30)
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                            .addComponent(Email)
                            .addComponent(AdEmailTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))))
                .addGap(100, 100, 100)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(AdRegBtn)
                    .addComponent(goBackBtn))
                .addGap(27, 27, 27))
        );

        pack();
    }// </editor-fold>                        

    private void AdRegBtnActionPerformed(java.awt.event.ActionEvent evt) {                                         
       connection = MySqlConnect.ConnectDB();
       
        try {        
               String query = "INSERT INTO admin_infosave (?First_name, Last_name, Corporate_email, Password) VALUES (?,?,?,?)";
               pstmt = connection.prepareStatement(query);
               pstmt.setString(1, AdFNTextBox.getText());
               pstmt.setString(2, AdLNTextBox.getText());
               pstmt.setString(3, AdEmailTextBox.getText());
               pstmt.setString(4, Arrays.toString(AdPassTextBox.getPassword()));
               
               //Once the user clicks the Register button, the user will be registered and the information will be stored in the relevant database.
               
               pstmt.executeUpdate();
               JOptionPane.showMessageDialog(null, "You have been registered.");
               this.setVisible(false);
               new AdminHomeScreen().setVisible(true);
   
        } catch (Exception e) {
           JOptionPane.showMessageDialog(null, e);
       }
    }                                        

    private void AdEmailTextBoxActionPerformed(java.awt.event.ActionEvent evt) {                                               
        // TODO add your handling code here:
    }                                              

    private void goBackBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.setVisible(false);
        new WelcomeScreen().setVisible(true);
    }                                         

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(AdminRegisterScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(AdminRegisterScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(AdminRegisterScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(AdminRegisterScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new AdminRegisterScreen().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify                     
    private javax.swing.JTextField AdEmailTextBox;
    private javax.swing.JTextField AdFNTextBox;
    private javax.swing.JTextField AdLNTextBox;
    private javax.swing.JPasswordField AdPassTextBox;
    private javax.swing.JButton AdRegBtn;
    private javax.swing.JLabel Email;
    private javax.swing.JLabel FirstName;
    private javax.swing.JLabel LastName;
    private javax.swing.JLabel Password;
    private javax.swing.JButton goBackBtn;
    // End of variables declaration                   
}

AdminRegStudentScreen

    Connection connection = MySqlConnect.ConnectDB();
    PreparedStatement pstmt = null;
    ResultSet rs = null;
    
    public AdminRegStudentScreen() {
        initComponents();
    }
    
    public void regStudent() {
        try {
        String regUser = "INSERT INTO student_infosave (First_name, Last_name, University, Degree_type) VALUES (?,?,?,?)";
        pstmt = connection.prepareStatement(regUser);
        
        pstmt.setString(1, FNTextBox.getText());
        pstmt.setString(2, LNTextBox.getText());
        pstmt.setString(3, UniTextBox.getText());
        pstmt.setString(4, DegreeComboBox.getSelectedItem().toString());
        
        pstmt.executeUpdate();

        JOptionPane.showMessageDialog(null, "You have registered the user successfully.");
        this.setVisible(false);
        } catch (SQLException e) {
            JOptionPane.showMessageDialog(null, e);
        }
    }


AdminRegisterScreen

    private void AdRegBtnActionPerformed(java.awt.event.ActionEvent evt) {                                         
       connection = MySqlConnect.ConnectDB();
       
        try {        
               String query = "INSERT INTO admin_infosave (?First_name, Last_name, Corporate_email, Password) VALUES (?,?,?,?)";
               pstmt = connection.prepareStatement(query);
               pstmt.setString(1, AdFNTextBox.getText());
               pstmt.setString(2, AdLNTextBox.getText());
               pstmt.setString(3, AdEmailTextBox.getText());
               pstmt.setString(4, Arrays.toString(AdPassTextBox.getPassword()));
               
               //Once the user clicks the Register button, the user will be registered and the information will be stored in the relevant database.
               
               pstmt.executeUpdate();
               JOptionPane.showMessageDialog(null, "You have been registered.");
               this.setVisible(false);
               new AdminHomeScreen().setVisible(true);
   
        } catch (Exception e) {
           JOptionPane.showMessageDialog(null, e);
       }
    }   

    private void goBackBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.setVisible(false);
        new WelcomeScreen().setVisible(true);
    }    



EarningCalculatorScreen

package smart.budgeting.app;

import DatabaseConnection.MySqlConnect;
import Smart.Budgeting.App.HomeScreen;
import User.StudentController;
import java.io.*;
import java.sql.*;
import javax.imageio.ImageIO;
import javax.swing.ImageIcon;
import javax.swing.JFileChooser;
import javax.swing.JOptionPane;
import javax.swing.filechooser.FileNameExtensionFilter;

/**
 *
 * @author Ahmed A. Ali
 */

public class EarningCalculatorScreen extends javax.swing.JFrame {
    Connection connection = MySqlConnect.ConnectDB();
    PreparedStatement pstmt = null;
    ResultSet rs = null;
    StudentController controller;
    
    LoginScreen login;
    SalaryRecordsScreen salRecords;

    public EarningCalculatorScreen() {
        initComponents();
        
        //The editable areas have been locked to prevent the user from manipulating the values.
        CalculateTextArea.setEditable(false);
        smallestSalaryTextBox.setEditable(false);
        highestSalaryTextBox.setEditable(false);
        year5MinSalaryTextBox.setEditable(false);
        year5MaxSalaryTextBox.setEditable(false);
    }

    
    public void saveText() { //The user can save the details generated from the app as a .txt file
        FileNameExtensionFilter filterFile = new FileNameExtensionFilter("Text File", ".txt");
        final JFileChooser saveFile = new JFileChooser();
        saveFile.setApproveButtonText("Save");
        saveFile.setFileFilter(filterFile);
        int actionDialog = saveFile.showOpenDialog(this);
        if (actionDialog != JFileChooser.APPROVE_OPTION) {
            return;
        }
        
        File file = saveFile.getSelectedFile();
        if(!file.getName().endsWith(".txt")); {
            file = new File(file.getAbsolutePath() + ".txt");
        }
        
        BufferedWriter sendFile = null;
        try {
            sendFile = new BufferedWriter(new FileWriter(file));
            CalculateTextArea.write(sendFile);
            
        } catch (IOException e) {
            JOptionPane.showMessageDialog(null, e);
        } finally {
            if (sendFile != null) {
                try {
                    sendFile.close();
                } catch (IOException e) {
                    JOptionPane.showMessageDialog(null, e);
                }
            }
        }
        
    }

    
    //NOTE : 'The City University' = City, University of London
    public void calculateComputerScience() { //these voids will collect the data from the SQL database.
         try {
            String getCS = "SELECT lowerAnnEarn, upperAnnEarn FROM sfr18_17data data WHERE subject = 'Computer Science' AND providerName = ? LIMIT 1";
            pstmt = connection.prepareStatement(getCS);
            pstmt.setString(1, selectInstitutionComboBox.getSelectedItem().toString());
            rs = pstmt.executeQuery();            
            
            while (rs.next()) {
            smallestSalaryTextBox.setText(rs.getString("lowerAnnEarn"));
            highestSalaryTextBox.setText(rs.getString("upperAnnEarn"));
            }
        } catch (Exception e) {
            e.printStackTrace();
            JOptionPane.showMessageDialog(null, "Cannot retrieve from database.");
        }
         
     }
    
    public void calculateArt() {
        try {
            String getArt = "SELECT lowerAnnEarn, upperAnnEarn FROM sfr18_17data data WHERE subject = 'Creative arts & design' AND providerName = ? LIMIT 1";
            pstmt = connection.prepareStatement(getArt);
            pstmt.setString(1, selectInstitutionComboBox.getSelectedItem().toString());
            rs = pstmt.executeQuery();
           
            while (rs.next()) {
                smallestSalaryTextBox.setText(rs.getString("lowerAnnEarn"));
                highestSalaryTextBox.setText(rs.getString("upperAnnEarn"));
            }
        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, e);
        }        
    }
    
    public void calculateAgriculture() {
        try {
            String getAgriculture = "SELECT lowerAnnEarn, upperAnnEarn FROM sfr18_17data data WHERE subject = 'Agriculture & related subjects' AND providerName = ? LIMIT 1";
            pstmt = connection.prepareStatement(getAgriculture);
            pstmt.setString(1, selectInstitutionComboBox.getSelectedItem().toString());
            rs = pstmt.executeQuery();
            
            while (rs.next()) {
            smallestSalaryTextBox.setText(rs.getString("lowerAnnEarn"));
            highestSalaryTextBox.setText(rs.getString("upperAnnEarn"));
            }
        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, e);
        }       
    }
    
    public void calculateAgricultureCity() {
        try {
        String getAgricultureCS = "SELECT lowerAnnEarn, upperAnnEarn FROM sfr18_17data WHERE subject = 'Agriculture & related subjects' AND providerName = 'The City University'";
        pstmt = connection.prepareStatement(getAgricultureCS);
        pstmt.executeQuery();
        
        while (rs.next()) {
            smallestSalaryTextBox.setText(rs.getString("lowerAnnEarn"));
            highestSalaryTextBox.setText(rs.getString("upperAnnEarn"));            
        }
        
        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, e);
        }
    }    
    
    public void calculateArchitecture() {
        try {
            String getArchitecture = "SELECT lowerAnnEarn, upperAnnEarn FROM sfr18_17data data WHERE subject = 'Architecture building & planning' AND providerName = ? LIMIT 1";
            pstmt = connection.prepareStatement(getArchitecture);
            pstmt.setString(1, selectInstitutionComboBox.getSelectedItem().toString());
            rs = pstmt.executeQuery();
            
            while (rs.next()) {
            smallestSalaryTextBox.setText(rs.getString("lowerAnnEarn"));
            highestSalaryTextBox.setText(rs.getString("upperAnnEarn"));
            }
        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, e);
        }  
    }
    
    public void calculateBiological() {
        try {
            String getBio2 = "SELECT lowerAnnEarn, upperAnnEarn FROM sfr18_17data data WHERE subject = 'Biological sciences' AND providerName = ? LIMIT 1";
            pstmt = connection.prepareStatement(getBio2);
            pstmt.setString(1, selectInstitutionComboBox.getSelectedItem().toString());
            rs = pstmt.executeQuery();
            
            while (rs.next()) {
            smallestSalaryTextBox.setText(rs.getString("lowerAnnEarn"));
            highestSalaryTextBox.setText(rs.getString("upperAnnEarn"));
            }
            
        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, e);
        }  
    }
    
    public void calculateBusiness() {
        try {
            String getBusiness2 = "SELECT lowerAnnEarn, upperAnnEarn FROM sfr18_17data data WHERE subject = 'Business & administrative studies' AND providerName = ? LIMIT 1";
            pstmt = connection.prepareStatement(getBusiness2);
            pstmt.setString(1, selectInstitutionComboBox.getSelectedItem().toString());
            pstmt.executeQuery();
            
            while (rs.next()) {
            smallestSalaryTextBox.setText(rs.getString("lowerAnnEarn"));
            highestSalaryTextBox.setText(rs.getString("upperAnnEarn"));
            }
            
        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, e);
        }       
    }
                        

    private void saveDataBtnActionPerformed(java.awt.event.ActionEvent evt) {                                            
        this.saveText();
    }                                           

    private void goBackBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.setVisible(false);
        new HomeScreen().setVisible(true);
    }                                         

    private void calculateBtnActionPerformed(java.awt.event.ActionEvent evt) {                                             
        int lowSalary = Integer.parseInt(smallestSalaryTextBox.getText());
        int highSalary = Integer.parseInt(highestSalaryTextBox.getText());
        
        ImageIcon testGraph = new ImageIcon("C:\\Users\\Ahmed Ali\\Documents\\City University London\\BSc Business Computing Systems\\Year 3\\IN3007 - Individual Project\\Test graphs\\City, University of London test graph.png");
        String getUni = selectInstitutionComboBox.getSelectedItem().toString();
        String getSubject = degreeComboBox.getSelectedItem().toString();
        int calculate1 = lowSalary * 12;
        year5MinSalaryTextBox.setText(Integer.toString(calculate1));
        
        int calculate2 = highSalary * 12;
        year5MaxSalaryTextBox.setText(Integer.toString(calculate2));
        
        //the values from the text boxes will be stored in the text area for the user to save as a text file.

        CalculateTextArea.setText("Institution: " + getUni + "\n" + "Subject: " + getSubject + "\n" + "Minimum salary after 5 years: £" +  year5MinSalaryTextBox.getText() + "\n" + "Highest salary after 5 years: £" + year5MaxSalaryTextBox.getText());
        ImageArea.insertIcon(testGraph);
    }                                            

    private void degreeComboBoxActionPerformed(java.awt.event.ActionEvent evt) {                                               
        //these voids will reference earlier voids to get data from the SQL database to the text fields in the app.
        switch (degreeComboBox.getSelectedItem().toString()) {
            case "Computer Science":
                this.calculateComputerScience();
                break;
            case "Agriculture & Related Subjects":
                this.calculateAgriculture();
                break;
            case "Architecture Building & Planning":
                this.calculateArchitecture();
                break;
            case "Biological Sciences":
                this.calculateBiological();
                break;
            case "Business & Administrative Studies":
                this.calculateBusiness();
                break;
            case "Creative Art & Design":
                this.calculateArt();
                break;
            default:
                break;
        }
        
    }                                              


ForgotPassScreen

    private void submitBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        if (FPassEmailTextBox.getText().hashCode() != 0) {
            JOptionPane.showMessageDialog(this, "Please check your email for a confirmation letter to change your password.");
        } else {
            JOptionPane.showMessageDialog(this, "Please enter your email.");
        }
    }                                         

    private void goBackBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.setVisible(false);
        new WelcomeScreen().setVisible(true);
    }   

HomeScreen

package Smart.Budgeting.App;

import smart.budgeting.app.EarningCalculatorScreen;
import smart.budgeting.app.StudentFormScreen;
import smart.budgeting.app.WelcomeScreen;
import smart.budgeting.app.SalaryRecordsScreen;

public class HomeScreen extends javax.swing.JFrame {

    
    public HomeScreen() {
        initComponents();
    }

    /**
     * This method is called from within the constructor to initialize the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        jPanel1 = new javax.swing.JPanel();
        WelcomeLabel = new javax.swing.JLabel();
        earningBtn = new javax.swing.JButton();
        detailsBtn = new javax.swing.JButton();
        logOutBtn = new javax.swing.JButton();
        searchJobBtn = new javax.swing.JButton();
        ImageLabel = new javax.swing.JLabel();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        jPanel1.setBackground(new java.awt.Color(51, 51, 255));
        jPanel1.setPreferredSize(new java.awt.Dimension(809, 451));
        jPanel1.setLayout(new org.netbeans.lib.awtextra.AbsoluteLayout());

        WelcomeLabel.setFont(new java.awt.Font("Gadugi", 1, 36)); // NOI18N
        WelcomeLabel.setForeground(new java.awt.Color(255, 255, 255));
        WelcomeLabel.setText("Welcome");
        jPanel1.add(WelcomeLabel, new org.netbeans.lib.awtextra.AbsoluteConstraints(70, 40, -1, -1));

        earningBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        earningBtn.setText("Calculate your earnings");
        earningBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                earningBtnActionPerformed(evt);
            }
        });
        jPanel1.add(earningBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(80, 190, 222, 138));

        detailsBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        detailsBtn.setText("Manage your details");
        detailsBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                detailsBtnActionPerformed(evt);
            }
        });
        jPanel1.add(detailsBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(320, 190, 210, 138));

        logOutBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        logOutBtn.setText("Log out");
        logOutBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                logOutBtnActionPerformed(evt);
            }
        });
        jPanel1.add(logOutBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(690, 50, -1, 35));

        searchJobBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        searchJobBtn.setText("View salary records");
        searchJobBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                searchJobBtnActionPerformed(evt);
            }
        });
        jPanel1.add(searchJobBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(540, 190, 222, 138));

        ImageLabel.setIcon(new javax.swing.ImageIcon("C:\\Users\\Ahmed Ali\\Documents\\City University London\\BSc Business Computing Systems\\Year 3\\IN3007 - Individual Project\\Smart Budgeting App (Final Year Project)\\src\\smart\\budgeting\\app\\Spray-pattern-image.png")); // NOI18N
        ImageLabel.setMaximumSize(new java.awt.Dimension(900, 900));
        jPanel1.add(ImageLabel, new org.netbeans.lib.awtextra.AbsoluteConstraints(0, -20, 840, 500));

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addComponent(jPanel1, javax.swing.GroupLayout.DEFAULT_SIZE, 840, Short.MAX_VALUE)
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addComponent(jPanel1, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
        );

        pack();
    }// </editor-fold>                        

    private void earningBtnActionPerformed(java.awt.event.ActionEvent evt) {                                           
        this.setVisible(false);
        new EarningCalculatorScreen().setVisible(true);
    }                                          

    private void logOutBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.setVisible(false);
        new WelcomeScreen().setVisible(true);
    }                                         

    private void searchJobBtnActionPerformed(java.awt.event.ActionEvent evt) {                                             
        this.setVisible(false);
        new SalaryRecordsScreen().setVisible(true);
    }                                            

    private void detailsBtnActionPerformed(java.awt.event.ActionEvent evt) {                                           
        this.setVisible(false);
        new StudentFormScreen().setVisible(true);
    }                                          

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(HomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(HomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(HomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(HomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new HomeScreen().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify                     
    private javax.swing.JLabel ImageLabel;
    private javax.swing.JLabel WelcomeLabel;
    private javax.swing.JButton detailsBtn;
    private javax.swing.JButton earningBtn;
    private javax.swing.JPanel jPanel1;
    private javax.swing.JButton logOutBtn;
    private javax.swing.JButton searchJobBtn;
    // End of variables declaration                   
}


LoginScreen

    public void loginUser() {
        connection = MySqlConnect.ConnectDB();
        String email = EmailTextBox.getText();
        String pass = Arrays.toString(PassTextBox.getPassword());
        
        try {
            String query = "SELECT Email, Password FROM student_infosave";            
            pstmt = connection.prepareStatement(query);
            rs = pstmt.executeQuery();
           
            while (rs.next()) {
                if (email.equals(rs.getString("Email"))) {
                if (pass.equals(rs.getString("Password"))) {
                JOptionPane.showMessageDialog(null, "You have logged in successfully.");
                this.setVisible(false);
                new HomeScreen().setVisible(true);
            } else {
                PassTextBox.setText("");
                JOptionPane.showMessageDialog(null, "Email or password details are incorrect. Please try again.");
            }
            }
            }
            
        } catch (SQLException | HeadlessException e) {
            JOptionPane.showMessageDialog(null, e);
        }
        
    }
    
            
    @SuppressWarnings("InfiniteRecursion")
    public String getEmail() {
        this.EmailTextBox.getText();
        return getEmail();
    } 

    private void forgotPassBtnActionPerformed(java.awt.event.ActionEvent evt) {                                              
        this.setVisible(false);
        new ForgotPassScreen().setVisible(true);
    }                                             

    private void loginBtnActionPerformed(java.awt.event.ActionEvent evt) {                                         
        this.loginUser();
    }      

    private void PassTextBoxKeyPressed(java.awt.event.KeyEvent evt) {                                       
        if(evt.getKeyCode() == KeyEvent.VK_ENTER) {
            this.loginUser();
        }
    }       

RegisterScreen

package smart.budgeting.app;

import DatabaseConnection.MySqlConnect;
import Smart.Budgeting.App.HomeScreen;
import User.StudentController;
import java.sql.*;
import java.awt.HeadlessException;
import java.awt.event.KeyEvent;
import java.util.Arrays;
import javax.swing.JOptionPane;

/**
 *
 * @author Ahmed A. Ali
 */
public class LoginScreen extends javax.swing.JFrame {

    Connection connection = null;
    PreparedStatement pstmt = null;
    ResultSet rs = null;
    StudentController controller;
    
    public LoginScreen() {
        initComponents();
    }

    public void loginUser() {
        connection = MySqlConnect.ConnectDB();
        String email = EmailTextBox.getText();
        String pass = Arrays.toString(PassTextBox.getPassword());
        
        try {
            String query = "SELECT Email, Password FROM student_infosave";            
            pstmt = connection.prepareStatement(query);
            rs = pstmt.executeQuery();
           
            while (rs.next()) {
                if (email.equals(rs.getString("Email"))) {
                if (pass.equals(rs.getString("Password"))) {
                JOptionPane.showMessageDialog(null, "You have logged in successfully.");
                this.setVisible(false);
                new HomeScreen().setVisible(true);
            } else {
                PassTextBox.setText("");
                JOptionPane.showMessageDialog(null, "Email or password details are incorrect. Please try again.");
            }
            }
            }
            
        } catch (SQLException | HeadlessException e) {
            JOptionPane.showMessageDialog(null, e);
        }
        
    }
    
            
    @SuppressWarnings("InfiniteRecursion")
    public String getEmail() {
        this.EmailTextBox.getText();
        return getEmail();
    }  
    /**
     * This method is called from within the constructor to initialize the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        forgotPassBtn = new javax.swing.JButton();
        EmailTextBox = new javax.swing.JTextField();
        Email = new javax.swing.JLabel();
        Password = new javax.swing.JLabel();
        loginBtn = new javax.swing.JButton();
        PassTextBox = new javax.swing.JPasswordField();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        forgotPassBtn.setFont(new java.awt.Font("Gadugi", 1, 11)); // NOI18N
        forgotPassBtn.setText("Forgot Password?");
        forgotPassBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                forgotPassBtnActionPerformed(evt);
            }
        });

        EmailTextBox.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        EmailTextBox.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                EmailTextBoxActionPerformed(evt);
            }
        });

        Email.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        Email.setText("Email");

        Password.setFont(new java.awt.Font("Gadugi", 0, 14)); // NOI18N
        Password.setText("Password");

        loginBtn.setFont(new java.awt.Font("Gadugi", 1, 11)); // NOI18N
        loginBtn.setText("Login");
        loginBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                loginBtnActionPerformed(evt);
            }
        });
        loginBtn.addKeyListener(new java.awt.event.KeyAdapter() {
            public void keyPressed(java.awt.event.KeyEvent evt) {
                loginBtnKeyPressed(evt);
            }
        });

        PassTextBox.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        PassTextBox.addKeyListener(new java.awt.event.KeyAdapter() {
            public void keyPressed(java.awt.event.KeyEvent evt) {
                PassTextBoxKeyPressed(evt);
            }
        });

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGap(56, 56, 56)
                .addComponent(loginBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 100, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, 400, Short.MAX_VALUE)
                .addComponent(forgotPassBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 146, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(58, 58, 58))
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addContainerGap(javax.swing.GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addComponent(Email)
                    .addComponent(Password))
                .addGap(101, 101, 101)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING, false)
                    .addComponent(EmailTextBox, javax.swing.GroupLayout.DEFAULT_SIZE, 191, Short.MAX_VALUE)
                    .addComponent(PassTextBox))
                .addGap(216, 216, 216))
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addGap(116, 116, 116)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.TRAILING)
                    .addGroup(layout.createSequentialGroup()
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                            .addComponent(EmailTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addComponent(Email))
                        .addGap(44, 44, 44)
                        .addComponent(Password))
                    .addComponent(PassTextBox, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, 87, Short.MAX_VALUE)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(loginBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 40, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(forgotPassBtn, javax.swing.GroupLayout.PREFERRED_SIZE, 40, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addGap(31, 31, 31))
        );

        pack();
    }// </editor-fold>                        

    private void forgotPassBtnActionPerformed(java.awt.event.ActionEvent evt) {                                              
        this.setVisible(false);
        new ForgotPassScreen().setVisible(true);
    }                                             

    private void EmailTextBoxActionPerformed(java.awt.event.ActionEvent evt) {                                             

    }                                            

    private void loginBtnActionPerformed(java.awt.event.ActionEvent evt) {                                         
        this.loginUser();
    }                                        

    private void loginBtnKeyPressed(java.awt.event.KeyEvent evt) {                                    

    }                                   

    private void PassTextBoxKeyPressed(java.awt.event.KeyEvent evt) {                                       
        if(evt.getKeyCode() == KeyEvent.VK_ENTER) {
            this.loginUser();
        }
    }                                      

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(LoginScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(LoginScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(LoginScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(LoginScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new LoginScreen().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify                     
    private javax.swing.JLabel Email;
    public javax.swing.JTextField EmailTextBox;
    public javax.swing.JPasswordField PassTextBox;
    private javax.swing.JLabel Password;
    private javax.swing.JButton forgotPassBtn;
    private javax.swing.JButton loginBtn;
    // End of variables declaration                   
}

StudentFormScreen (may contain errors)

    public void retrieveDetails() {
        try {        
            String getDetails = "SELECT First_name, Last_name, Email, University, Degree_type, Password FROM student_infosave WHERE ID = ? ";
            pstmt = connection.prepareStatement(getDetails);
            pstmt.executeQuery(getDetails);
        
            String getFName = rs.getString("First_name");
            String getLName = rs.getString("Last_name");
            String getEmail = rs.getString("Email");
            String getUniversity = rs.getString("University");
            String getDegreeType = rs.getString("Degree_type");
            String getPassword = rs.getString("Password");
            
            
            UGFNTextBox.setText(getFName);
            UGLNTextBox.setText(getLName);
            UGEmailTextBox.setText(getEmail);
            UGUniTextBox.setText(getUniversity);
            UGDegreeTypeComboBox.setSelectedItem(getDegreeType);
            UGPassTextBox.setText(getPassword);
            
        } catch (Exception e) {
            e.printStackTrace();
            JOptionPane.showMessageDialog(null, "Cannot retrieve details from database.");
        }
    }

    public void setNewDetails() {
        try {
            String updateDetails = "UPDATE student_infosave"
                    + "SET Email = '"+UGEmailTextBox.getText()+"' + WHERE First_name = '"+UGFNTextBox.getText()+"'"
                    + " SET University = '"+UGUniTextBox.getText()+"'+ WHERE First_name = '"+UGFNTextBox.getText()+"'"
                    + " SET Degree_type = '"+UGDegreeTypeComboBox.getSelectedItem().toString()+"'+ WHERE First_name = '"+UGFNTextBox.getText()+"'" 
                    + " SET Password = '"+UGPassTextBox.getText()+"'+ WHERE First_name = '"+UGFNTextBox.getText()+"'";
            
            pstmt = connection.prepareStatement(updateDetails);
            pstmt.executeUpdate();

        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, e);
        }
    }
    
    public void getCorrectDetails() {
        if (login.EmailTextBox.equals(rs)) {        
            try {
                UGEmailTextBox.setText(rs.getString("Email"));       
            } catch (Exception e) {
                JOptionPane.showMessageDialog(null, e);  
            }
        }
    }

    private void goBackBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.setVisible(false);
        new HomeScreen().setVisible(true);
    }       

WelcomeScreen

package smart.budgeting.app;


public class WelcomeScreen extends javax.swing.JFrame {


    public WelcomeScreen() {
        initComponents();
    }

    /**
     * This method is called from within the constructor to initialise the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    
    // Image from Business Insider UK: http://uk.businessinsider.com/the-most-beautiful-university-campuses-in-the-uk-2016-8
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        StuLoginBtn = new javax.swing.JButton();
        StuRegBtn = new javax.swing.JButton();
        AdmLoginBtn = new javax.swing.JButton();
        AdmRegBtn = new javax.swing.JButton();
        jLabel2 = new javax.swing.JLabel();
        jLabel1 = new javax.swing.JLabel();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);
        getContentPane().setLayout(new org.netbeans.lib.awtextra.AbsoluteLayout());

        StuLoginBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        StuLoginBtn.setText("Student Login");
        StuLoginBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                StuLoginBtnActionPerformed(evt);
            }
        });
        getContentPane().add(StuLoginBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(520, 250, 124, 49));

        StuRegBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        StuRegBtn.setText("Student Register");
        StuRegBtn.addMouseListener(new java.awt.event.MouseAdapter() {
            public void mouseClicked(java.awt.event.MouseEvent evt) {
                StuRegBtnMouseClicked(evt);
            }
        });
        StuRegBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                StuRegBtnActionPerformed(evt);
            }
        });
        getContentPane().add(StuRegBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(520, 300, 124, 49));

        AdmLoginBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        AdmLoginBtn.setText("Admin Login");
        AdmLoginBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                AdmLoginBtnActionPerformed(evt);
            }
        });
        getContentPane().add(AdmLoginBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(150, 250, 124, 49));

        AdmRegBtn.setFont(new java.awt.Font("Gadugi", 0, 11)); // NOI18N
        AdmRegBtn.setText("Admin Register");
        AdmRegBtn.addMouseListener(new java.awt.event.MouseAdapter() {
            public void mouseClicked(java.awt.event.MouseEvent evt) {
                AdmRegBtnMouseClicked(evt);
            }
        });
        AdmRegBtn.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                AdmRegBtnActionPerformed(evt);
            }
        });
        getContentPane().add(AdmRegBtn, new org.netbeans.lib.awtextra.AbsoluteConstraints(150, 300, 124, 49));

        jLabel2.setIcon(new javax.swing.ImageIcon(getClass().getResource("/smart/budgeting/app/GradGO logo.png"))); // NOI18N
        getContentPane().add(jLabel2, new org.netbeans.lib.awtextra.AbsoluteConstraints(0, 0, 760, 360));

        jLabel1.setIcon(new javax.swing.ImageIcon("C:\\Users\\Ahmed Ali\\Documents\\City University London\\BSc Business Computing Systems\\Year 3\\IN3007 - Individual Project\\University image UK.jpg")); // NOI18N
        getContentPane().add(jLabel1, new org.netbeans.lib.awtextra.AbsoluteConstraints(0, 1, 760, 360));

        pack();
    }// </editor-fold>                        

    private void AdmLoginBtnActionPerformed(java.awt.event.ActionEvent evt) {                                            
        this.dispose();
        new AdminLoginScreen().setVisible(true);
    }                                           

    private void StuLoginBtnActionPerformed(java.awt.event.ActionEvent evt) {                                            
        this.dispose();
        new LoginScreen().setVisible(true);
    }                                           

    private void StuRegBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.dispose();
        new RegisterScreen().setVisible(true);
    }                                         

    private void StuRegBtnMouseClicked(java.awt.event.MouseEvent evt) {                                       
        this.dispose();
        new RegisterScreen().setVisible(true);
    }                                      

    private void AdmRegBtnMouseClicked(java.awt.event.MouseEvent evt) {                                       
        this.dispose();
        new AdminRegisterScreen().setVisible(true);
    }                                      

    private void AdmRegBtnActionPerformed(java.awt.event.ActionEvent evt) {                                          
        this.dispose();
        new AdminRegisterScreen().setVisible(true);
    }                                         

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(WelcomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(WelcomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(WelcomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(WelcomeScreen.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(() -> {
            new WelcomeScreen().setVisible(true);
        });
    }

    // Variables declaration - do not modify                     
    private javax.swing.JButton AdmLoginBtn;
    private javax.swing.JButton AdmRegBtn;
    private javax.swing.JButton StuLoginBtn;
    private javax.swing.JButton StuRegBtn;
    private javax.swing.JLabel jLabel1;
    private javax.swing.JLabel jLabel2;
    // End of variables declaration                   
}

DataCollector
package DataMine;

import DatabaseConnection.MySqlConnect;
import Datasets.StudentDatasets;
import java.io.*;
import java.sql.*;
import java.util.*;
import java.text.*;

public class DataCollector {

    Connection connection = MySqlConnect.ConnectDB();
    PreparedStatement pstmt = null;
    ResultSet rs = null;
    
    public static void main(String [] args) {
        String COMMA_DELIMITER = ",";
        String NEW_LINE_SEPARATOR = "/n";
        String FILE_HEADER = "studentid, name, address, university, creationDate";
        try {
            SimpleDateFormat sdf = new SimpleDateFormat("dd-MM-yyyy");
            List<StudentDatasets> studentList = new ArrayList<StudentDatasets>();
            FileWriter fw = new FileWriter("src\\Data\\studentList.csv");
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
    
}



DataGenerator
package DataMine;

import java.util.*;

public class DataGenerator {
    private int studentid;
    private String name;
    private String address;
    private String university;
    private Date creationDate;
    
    public Date getCreationDate() {
        return creationDate;
    }
    
    public void setCreationDate(Date creationDate) {
        this.creationDate = creationDate;
    }
    
    public DataGenerator(int studentid,String name, String address, String university, Date creationDate) {
        super();
        this.studentid = studentid;
        this.name = name;
        this.address = address;
        this.university = university;
        this.creationDate = creationDate;
    }
    
    public DataGenerator() {
        super();
    }
}



GovDatasets
package Datasets;

import DatabaseConnection.*;
import java.io.*;
import java.net.*;
import java.sql.*;


public class GovDatasets extends Thread {
    
    private Thread t;
    private Connection connection;
    private PreparedStatement pstmt;
    private ResultSet rs;
    private String threadName;

    public class retrieveData {
        
        public void fetchData() {
            connection = MySqlConnect.ConnectDB();
        try {
            String getData = "SELECT * FROM sfr18_17data WHERE UKPRN = 10004078 AND yearsAfterGraduation <= 5"; //Data from: https://www.gov.uk/government/statistics/graduate-outcomes-for-all-subjects-by-university
            pstmt = connection.prepareStatement(getData);
            rs = pstmt.executeQuery();
        } catch(Exception e) {
            e.printStackTrace();
        }
        }
        
        public void getData(String webURL) throws IOException {
        URL url = new URL("C:\\Users\\Ahmed Ali\\Documents\\City University London\\BSc Business Computing Systems\\Year 3\\IN3007 - Individual Project\\Spend_Over_500_GBP_Current_Year.csv"); 
        //from http://data.gov.uk under the OGL (Open Govenment License): http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/
        URLConnection con = url.openConnection();
        InputStream is = con.getInputStream();
        
        try(BufferedReader br = new BufferedReader(new InputStreamReader(is))) {
        
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
        } catch (MalformedURLException e) {
            throw new MalformedURLException("Incorrect URL, try again!");
        } catch (IOException e) {
            throw new IOException();
        }
        }
        
        public void main(String [] args) throws IOException {
            String url = "http://stackoverflow.com/questions/6159118/using-java-to-pull-data-from-a-webpage\"";
            getData(url);
        }
        
        public void start() {
            System.out.println("Starting" + threadName);
            if (t == null) {
                t = new Thread((Runnable) this, threadName);
                t.start();
            }
        }
    }

    public GovDatasets() {
        
    }
}



StudentDatasets
package Datasets;

import java.io.*;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;


public class StudentDatasets {
    private Thread t;
    private String threadName;

    public class retrieveData {
        
        public void getData(String webURL) throws IOException {
        URL url = new URL("C:\\Users\\Ahmed Ali\\Documents\\City University London\\BSc Business Computing Systems\\Year 3\\IN3007 - Individual Project\\student-jan-17-transparency-data.csv"); 
        //from http://data.gov.uk under the OGL (Open Govenment License): http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/
        URLConnection con = url.openConnection();
        InputStream is = con.getInputStream();
        
        try(BufferedReader br = new BufferedReader(new InputStreamReader(is))) {
        
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
        } catch (MalformedURLException e) {
            throw new MalformedURLException("Cannot read URL. Please try again.");
        } catch (IOException e) {
            throw new IOException();
        }
        }
        
        public void main(String [] args) throws IOException {
            String url = "http://stackoverflow.com/questions/6159118/using-java-to-pull-data-from-a-webpage\"";
            getData(url);
        }
        
        public void start() {
            System.out.println("Starting" + threadName);
            if (t == null) {
                t = new Thread((Runnable) this, threadName);
                t.start();
            }
        }
    }

    public StudentDatasets() {
        String name;
        int setNo;
         
    }   
}


GraphGenerator



Report



AdminController
package User;

import java.sql.*;
import DatabaseConnection.MySqlConnect;

public class AdminController {
    Connection connection = MySqlConnect.ConnectDB();
    PreparedStatement pstmt = null;
    ResultSet rs = null;
    
    public void retrieveAdmin() {
        try {
            String getAdmin = "SELECT * FROM admin_infosave";
            pstmt = connection.prepareStatement(getAdmin);
            pstmt.executeQuery();
        } catch (Exception e) {
            e.printStackTrace();
        }  
    }
    
            
    public void manageStudent() {
        try {
            String retrieveStudent = "SELECT * FROM student_infosave";
            pstmt = connection.prepareStatement(retrieveStudent);
            pstmt.executeQuery();
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}



StudentController
package User;

import DatabaseConnection.MySqlConnect;
import java.sql.*;
import java.util.regex.*;
import javax.swing.JOptionPane;

public class StudentController {
 Connection connection = MySqlConnect.ConnectDB();
 PreparedStatement pstmt = null;
 ResultSet rs = null;
 
 
    public void isGraduate() {
        try {
        String getDegree = "SELECT Degree_type from student_infosave";
        pstmt = connection.prepareStatement(getDegree);
        pstmt.executeQuery();
        
        if (rs.getString("Degree_type") == null) {
            JOptionPane.showMessageDialog(null, "User is not a graduate.");
        }
        
        } catch (Exception e) {
            e.printStackTrace();
            JOptionPane.showMessageDialog(null, "Cannot retrieve from database.");
        }
    }
    
    public void retrieveStudent() {
        try {
            String getStudent = "SELECT * FROM student_infosave";
            pstmt = connection.prepareStatement(getStudent);
            pstmt.executeQuery();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    public boolean checkEmail(String email) {
        boolean isUniEmail = false;
        String emailPattern = "[_a-zA-Z0-9-\\+]+(\\.[_a-zA-Z0-9-]+)*@"+"[a-zA-Z0-9-]+(\\.[a-zA-Z0-9]+)*(\\.[a-zA-Z]{2,})$" + ".ac.uk";
        Pattern pattern = Pattern.compile(emailPattern);
        Matcher matcher = pattern.matcher(email);
        if (matcher.matches()) {
            isUniEmail = true;
        } else {
            isUniEmail = false;
        }
        return isUniEmail;
    }
}


ContactServer
package User;

import java.io.*;
import java.net.*;

public class ContactServer {
    ContactServer() {
        try {
            ServerSocket server = new ServerSocket(9090);
            
            while(true) {
                Socket socket = server.accept();
                SocketHandler sHandler = new SocketHandler(socket);
                Thread t = new Thread(sHandler);
                t.start();
            }
        } catch(IOException e) {
            e.printStackTrace();
        }
    }
    
    public static class SocketHandler implements Runnable {
        private final Socket socket;
        private final BufferedReader br;
        
        public SocketHandler(Socket socket) throws IOException {
            this.socket = socket;
            this.br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        }
        
        @Override
        public void run() {
            String email = null;    
        try {
            while ((email = br.readLine()) != null) {
                System.out.println("Incoming email : " + email);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        finally {
            try {
                br.close();
                socket.close();
            } catch(IOException e) {
                e.printStackTrace();
            }
        }
    }
    }
    
    public static void main(String args[]) {
        new ContactServer();
    }
}


User
package User;
import java.awt.Component;
import java.sql.*;

public class User {

    public static Component insertUserData;
    public String first_name;
    public String last_name;
    public String email;
    public String password;
    public String university;
    public String degree_type;


    
    public User(String fn, String ln, String p, String uni, String dt, String e) {
       first_name = fn;
       last_name = ln;
       password = p;
       university = uni;
       degree_type = dt;
       email = e;
    }
    
    public enum Status {
        UNDERGRADUATE(), POSTGRADUATE();
    }
    
    private Connection connect() {
        String url = "jdbc:mysql://localhost:3306/student_infosave";
        Connection conn = null;
        try {
            conn = DriverManager.getConnection(url);
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
        return conn;
    }
        public String addUserName (String userName) {
            String sql = "INSERT INTO student_infosave(`first name`) VALUES (userName)";
            
            try (Connection conn = this.connect()) {
                PreparedStatement stmt = conn.prepareStatement(sql);
                stmt.setString(1, userName);
                stmt.executeUpdate();
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        return userName;
        }
        
        public String getFirstName() {
            return first_name;
        }
        
        public String getLastName() {
            return last_name;
        }
        
        public String getEmail() {
            return email;
        }
        
        public String getUniversity() {
            return university;
        }
        
        public String getDegreeType() {
            return degree_type;
        }
        
        public String getPassword() {
            return password;
        }
    
}
