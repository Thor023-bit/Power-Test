# Power-Test
Time Stamp Testing
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO.Ports;
using System.IO;
using System.Windows.Forms;

namespace Life_Test_Jig
{
    public partial class Form1 : Form
    {
        private SerialPort Comport;
        private bool ComportOpen;
               

        public Form1()
        {
            InitializeComponent();
            ComportOpen = false;
            Comport = new SerialPort();
            Comport.DataReceived += new SerialDataReceivedEventHandler(HandleIncomingComportData);
            cbComport.Items.Clear();
            foreach (String Portname in System.IO.Ports.SerialPort.GetPortNames ())
            {
                cbComport.Items.Add(Portname);
            }
       
        }

        private void HandleIncomingComportData(object sender, SerialDataReceivedEventArgs e)
        {
            string MPF;
            MPF = Comport.ReadLine();
            AppendTextBox((DateTime.Now) + MPF);
            

        }
        public void AppendTextBox(string NewStr)
        {
            if (InvokeRequired)                                                                    // Handle messages from a different thread (comport)
            {
                this.Invoke(new Action<string>(AppendTextBox), new object[] { NewStr });
                return;
            }
            //tbDebugLog.Text += NewStr + "\n";
            tbDebugLog.AppendText(NewStr);
            tbDebugLog.AppendText(Environment.NewLine);
            //tbDebugLog.

        }
        private void checkBox1_CheckedChanged(object sender, EventArgs e)
        {

        }

        private void cbComportTorque_SelectedIndexChanged(object sender, EventArgs e)
        {

        }

        private void OpenPortTorque_Click(object sender, EventArgs e)
        {
            if (ComportOpen)
            {
                Comport.Close();
                ComportOpen = false;
            }
            Comport.PortName = cbComport.Text;
            Comport.BaudRate = 9600;
            Comport.Handshake = Handshake.None;
            Comport.ReadBufferSize = 4096;
            Comport.WriteTimeout = -1;
            Comport.ReadTimeout = -1;
            try
            {
                Comport.Open();
                ComportOpen = Comport.IsOpen;
                if (ComportOpen)
                {        
                    OpenPort.Enabled = false;                 
                }
            }
            catch
            {
                MessageBox.Show("Unable to open comport - please check if it is valid and not opened by another application & try again.");
               
            }

        }

        private void bnTest_Click(object sender, EventArgs e)
        {
            string text;
            text = "about";
            Comport.WriteLine(text);
        }
    }
    }

