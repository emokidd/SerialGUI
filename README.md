# SerialGUI
The other file required

import java.awt.BorderLayout;

import java.awt.EventQueue;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.Arrays;

import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.border.EmptyBorder;
import javax.swing.GroupLayout;
import javax.swing.GroupLayout.Alignment;
import javax.swing.JLabel;

import javax.swing.JTextField;

/**
 * Simple userinterface around the SerialComm class.
 * @author Fjodor van Slooten
 *
 */
public class SerialGUI extends JFrame {

	private JPanel contentPane;
	private JLabel label;
	private JLabel label2;
	private JLabel ud;
	private JLabel mr;
	private JTextField tf;
	ArrayList<String> ar = new ArrayList<String>();
	
	private JLabel title;
	/**
	 * Launch the application.
	 */
	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					SerialGUI frame = new SerialGUI();
					frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the frame.
	 */
	public SerialGUI() {
		setTitle("RFID reader");
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 800, 400);
		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
		setContentPane(contentPane);
		
		label = new JLabel("Please scan your badge");
		label2 = new JLabel();
		ud = new JLabel("Updates:");
		mr = new JLabel("Recent Medical Records:");
		tf = new JTextField();
		tf.setVisible(false);
		ud.setVisible(false);
		mr.setVisible(false);
		tf.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e){
				String input = tf.getText();
				ar.add(input);
				StringBuilder sb = new StringBuilder();
				for (String s : ar)
				{
				    sb.append(s);
				    sb.append("<br>");
				}
				label2.setText("<html>"+sb.toString()+"</html>");
				tf.setText("");				
			}
		});
//		contentPane.add(title);
//		title.setLocation(123,20);
		GroupLayout gl_contentPane = new GroupLayout(contentPane);
		gl_contentPane.setHorizontalGroup(
			gl_contentPane.createSequentialGroup()	
				.addComponent(label, GroupLayout.PREFERRED_SIZE, 150, GroupLayout.PREFERRED_SIZE)
				.addGroup(gl_contentPane.createParallelGroup(Alignment.LEADING)
					.addComponent(ud, GroupLayout.PREFERRED_SIZE, 150, GroupLayout.PREFERRED_SIZE)
					.addComponent(tf, GroupLayout.PREFERRED_SIZE, 150, GroupLayout.PREFERRED_SIZE))
				.addGroup(gl_contentPane.createParallelGroup(Alignment.LEADING)
					.addComponent(mr, GroupLayout.PREFERRED_SIZE, 150, GroupLayout.PREFERRED_SIZE)
					.addComponent(label2, GroupLayout.PREFERRED_SIZE, 150, GroupLayout.PREFERRED_SIZE))	
		);
		gl_contentPane.setVerticalGroup(
			gl_contentPane.createSequentialGroup()				
				.addGroup(gl_contentPane.createParallelGroup(Alignment.BASELINE)
					.addComponent(label, GroupLayout.PREFERRED_SIZE, 20, GroupLayout.PREFERRED_SIZE)
					.addComponent(ud, GroupLayout.PREFERRED_SIZE, 20, GroupLayout.PREFERRED_SIZE)
					.addComponent(mr, GroupLayout.PREFERRED_SIZE, 20, GroupLayout.PREFERRED_SIZE))
				.addComponent(tf, GroupLayout.PREFERRED_SIZE, 20, GroupLayout.PREFERRED_SIZE)
				.addComponent(label2, GroupLayout.PREFERRED_SIZE, 200, GroupLayout.PREFERRED_SIZE)
		);
		contentPane.setLayout(gl_contentPane);
		
		// Add serial commmunication:
		SerialComm t = new SerialComm();
		t.initialize(this);
	}
	
	/**
	 * This method will be called with the id if the SerialComm finds the keyword "Card UID"
	 * @param id
	 */
	public void cuid(String id) {
		System.out.println("cuid="+id+".");
		String welcome = "Welcome, ";
		if (id.equals("16 A9 8B AB")) {
//			System.out.println("kena liao");
			label.setText(welcome+"Doctor A");
			tf.setVisible(false);
			label2.setVisible(false);
			ud.setVisible(false);
			mr.setVisible(false);
		}
		else if (id.equals("26 59 84 A1")) {
			label.setText(welcome+"Patient");
			tf.setVisible(true);
			label2.setVisible(true);
			ud.setVisible(true);
			mr.setVisible(true);
		}
		else {
			label.setText(welcome+"Doctor B");
			tf.setVisible(false);
			label2.setVisible(false);
			ud.setVisible(false);
			mr.setVisible(false);
		}
	}
}
