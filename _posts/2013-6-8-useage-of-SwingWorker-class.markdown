---
layout: post
title: SwingWorker类的使用示例
---

当使用java Swing来构建GUI时，将线程与Swing一起使用时，必须遵循两个简单的原则： 

  - 如果一个动作需要花费很长时间，在一个独立的工作器线程中做这件事而不要在事件分配线程中做 
  - 除了事件分配线程，不要在任何线程中接触Swing组件（编程中通常称此为单一线程规则） 

针对一个有加载文本文件的命令和取消加载过程的命令的程序，该文件在一个单独的线程中加载。在读取文件的过程中，Open菜单项被禁用，Cancel菜单项为可用。读取每一行后，状态条中的线性计数器被更新。读取过程完成之后，Open菜单项重新变为可用，Cancel项被禁用，状态行文本置为Done。

以上的程序任务可通过SwingWorker轻易实现。覆盖doInBackground方法来完成耗时的工作，不时地调用publish来报告工作进度。以下是源码：

	import java.awt.BorderLayout;
	import java.awt.EventQueue;
	import java.awt.event.ActionEvent;
	import java.awt.event.ActionListener;
	import java.io.File;
	import java.io.FileInputStream;
	import java.util.List;
	import java.util.Scanner;
	import java.util.concurrent.CancellationException;
	import java.util.concurrent.ExecutionException;
	
	import javax.swing.JFileChooser;
	import javax.swing.JFrame;
	import javax.swing.JLabel;
	import javax.swing.JMenu;
	import javax.swing.JMenuBar;
	import javax.swing.JMenuItem;
	import javax.swing.JScrollPane;
	import javax.swing.JTextArea;
	import javax.swing.SwingWorker;
	
	public class SwingWorkerTest {
	
		public static void main(String[] args) {
			EventQueue.invokeLater(new Runnable() {
	
				@Override
				public void run() {
					JFrame frame = new SwingWorkerFrame();
					frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
					frame.setVisible(true);
				}
	
			});
		}
	}
	
	class SwingWorkerFrame extends JFrame {
		public SwingWorkerFrame() {
			chooser = new JFileChooser();
			chooser.setCurrentDirectory(new File("."));
	
			textArea = new JTextArea();
			add(new JScrollPane(textArea));
			setSize(DEFAULT_WIDTH, DEFAULT_HEIGHT);
	
			statusLine = new JLabel("");
			add(statusLine, BorderLayout.SOUTH);
	
			JMenuBar menuBar = new JMenuBar();
			setJMenuBar(menuBar);
	
			JMenu menu = new JMenu("File");
			menuBar.add(menu);
	
			openItem = new JMenuItem("Open");
			menu.add(openItem);
			openItem.addActionListener(new ActionListener() {
	
				@Override
				public void actionPerformed(ActionEvent event) {
					int result = chooser.showOpenDialog(null);
					if (result == JFileChooser.APPROVE_OPTION) {
						textArea.setText("");
						openItem.setEnabled(false);
						textReader = new TextReader(chooser.getSelectedFile());
						textReader.execute();
						cancelItem.setEnabled(true);
					}
				}
	
			});
	
			cancelItem = new JMenuItem("Cancle");
			menu.add(cancelItem);
			cancelItem.setEnabled(false);
			cancelItem.addActionListener(new ActionListener() {
	
				@Override
				public void actionPerformed(ActionEvent event) {
					textReader.cancel(true);
				}
	
			});
	
		}
	
		private class ProgressData {
			public int number;
			public String line;
		}
	
		private class TextReader extends SwingWorker<StringBuilder, ProgressData> {
			public TextReader(File file) {
				this.file = file;
			}
	
			@Override
			protected StringBuilder doInBackground() throws Exception {
				int lineNumber = 0;
				Scanner in = new Scanner(new FileInputStream(file));
				while (in.hasNextLine()) {
					String line = in.nextLine();
					lineNumber++;
					text.append(line);
					text.append("\n");
					ProgressData data = new ProgressData();
					data.number = lineNumber;
					data.line = line;
					publish(data);
					Thread.sleep(1);
				}
				return text;
			}
	
			@Override
			protected void done() {
				try {
					StringBuilder result = get();
					textArea.setText(result.toString());
					statusLine.setText("Done");
				} catch (InterruptedException e) {
	
				} catch (CancellationException e) {
					textArea.setText("");
					statusLine.setText("Cancelled");
				} catch (ExecutionException e) {
					statusLine.setText("" + e.getCause());
				}
				cancelItem.setEnabled(false);
				openItem.setEnabled(true);
			}
	
			@Override
			protected void process(List<SwingWorkerFrame.ProgressData> data) {
				if (isCancelled())
					return;
				StringBuilder b = new StringBuilder();
				statusLine.setText("" + data.get(data.size() - 1).number);
				for (ProgressData d : data) {
					b.append(d.line);
					b.append("\n");
				}
				System.out.println("process size: " + data.size());
				textArea.append(b.toString());
			}
	
			private File file;
			private StringBuilder text = new StringBuilder();
		}

		private JFileChooser chooser;
		private JTextArea textArea;
		private JLabel statusLine;
		private JMenuItem openItem;
		private JMenuItem cancelItem;
		private SwingWorker<StringBuilder, ProgressData> textReader;
	
		public static final int DEFAULT_WIDTH = 450;
		public static final int DEFAULT_HEIGHT = 350;
	}

以上摘自JAVA核心技术 卷I 基础知识（原书第8版）

**API说明：javax.swing.SwingWorker`<T,V>` 6**

 - abstract T doInBackground() 

	覆盖这一方法来执行后台的任务并返回这一工作的结果

 - void process(List`<V>` data) 

	覆盖这一方法来处理事件分配线程中的中间进度数据

 - void publish(V... data) 

	传递中间进度数据到事件分配线程。从doInBackground调用这一方法

 - void execute() 

	为工作器线程的执行预定这个工作器。

 - SwingWorker.StateValue getState() 

	得到这个工作器线程的状态，值为PENDING、STARTED或DONE之一。



