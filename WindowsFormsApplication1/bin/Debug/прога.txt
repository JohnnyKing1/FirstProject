using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.IO;
using System.Net;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        string a;
        private void button1_Click(object sender, EventArgs e)
        {
            a = textBox1.Text;
            // Создать запрос на URL.
            WebRequest request = WebRequest.Create(a);
            // Если этого требует сервер, установите учетные данные.
            request.Credentials = CredentialCache.DefaultCredentials;
            // Получите ответ.
            WebResponse response = request.GetResponse();
            // Показать статус.
            label1.Text=((HttpWebResponse)response).StatusDescription;
            // Получить поток, содержащий контент, возвращенный сервером
            // Блок using обеспечивает автоматическое закрытие потока.
            using (Stream dataStream = response.GetResponseStream())
            {
                // Откройте поток с помощью StreamReader для быстрого доступа.
                StreamReader reader = new StreamReader(dataStream);            
                // Прочтите содержание.
                string responseFromServer = reader.ReadToEnd();
                // Запись в файл
                using (StreamWriter sw = new StreamWriter("text.txt"))
                {
                    foreach (char dir in responseFromServer)
                    {
                        sw.Write(dir);
                    }
                }
                // Отобразите контент.
                textBox2.Text = responseFromServer;
            }
            // Закройте ответ.
            response.Close();
        }

    }
}
