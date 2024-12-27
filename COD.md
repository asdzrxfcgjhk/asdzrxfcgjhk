using Microsoft.SqlServer.Server;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;
using Google.Apis.Auth.OAuth2;
using Google.Apis.Auth.OAuth2.Responses;
using Google.Apis.Services;
using Google.Apis.Util.Store;
using System.IO;
using System.Threading;
using System.Data.SqlClient;
using System.Data.Entity;
using System.Windows.Markup;

namespace YutaPhe
{
    /// <summary>
    /// Логика взаимодействия для MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void ExitButton_MouseDown(object sender, MouseButtonEventArgs e)
        {
            this.Close();
        }

        private void MinButton_MouseDown(object sender, MouseButtonEventArgs e)
        {
            this.WindowState = WindowState.Minimized;
        }

        private void Window_MouseDown(object sender, MouseButtonEventArgs e)
        {
            if (e.ChangedButton == MouseButton.Left)
            {
                this.DragMove();
            }
        }

        private void TextBox_TextChanged(object sender, TextChangedEventArgs e)
        {

        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            Login secondWindow = new Login();
            secondWindow.Show();
            this.Close();
        }

        private void CheckBox_Checked(object sender, RoutedEventArgs e)
        {

        }

        private void Tb2_PasswordChanged(object sender, RoutedEventArgs e)
        {
            // Скрываем текст-подсказку, когда пользователь начинает вводить пароль
            tbPlaceholder.Visibility = string.IsNullOrEmpty(tb2.Password) ? Visibility.Visible : Visibility.Collapsed;
        }


        private void TbPasswordVisible_TextChanged(object sender, TextChangedEventArgs e)
        {

        }

        private void tbPasswordVisible_TextChanged_1(object sender, TextChangedEventArgs e)
        {

        }

        private void tb2_PasswordChanged(object sender, RoutedEventArgs e)
        {
            // Обновляем текст в TextBox при изменении пароля
            if (!showPasswordCheckBox.IsChecked.GetValueOrDefault())
            {
                tbPasswordVisible.Text = tb2.Password; // Сохраняем пароль для отображения

            }
        }

        private void ShowPasswordCheckBox_Checked(object sender, RoutedEventArgs e)
        {
            // Скрываем PasswordBox и показываем TextBox
            tbPasswordVisible.Text = tb2.Password; // Копируем пароль
            tb2.Visibility = Visibility.Collapsed;
            tbPasswordVisible.Visibility = Visibility.Visible;
        }

        private void ShowPasswordCheckBox_Unchecked(object sender, RoutedEventArgs e)
        {
            // Скрываем TextBox и показываем PasswordBox
            tb2.Password = tbPasswordVisible.Text; // Копируем текст обратно в PasswordBox
            tbPasswordVisible.Visibility = Visibility.Collapsed;
            tb2.Visibility = Visibility.Visible;
        }

        private void Button_Click_1(object sender, RoutedEventArgs e)
        {

        }

        private static string[] Scopes = { "https://www.googleapis.com/auth/userinfo.email"};
        private async void GoogleLoginButton_Click(object sender, RoutedEventArgs e)
        {
            UserCredential credential;

            // Загрузка файла client_secret.json
            using (var stream = new FileStream("client_apps.googleusercontent.com.json", FileMode.Open, FileAccess.Read))
            {
                // Путь для хранения токена
                var credPath = "token.json";

                // Использование FromStream вместо Load
                credential = await GoogleWebAuthorizationBroker.AuthorizeAsync(
                    GoogleClientSecrets.FromStream(stream).Secrets,
                    Scopes, // Убедитесь, что Scopes определены
                    "user",
                    CancellationToken.None,
                    new FileDataStore(credPath, true));
            }

            // Успешный вход
            MessageBox.Show($"Добро пожаловать, {credential.UserId}");
            Main secondWindow = new Main();
            secondWindow.Show();
            this.Close();
        }

        private void TransparentButton_Click(object sender, RoutedEventArgs e)
        {
            string connectionString = @"Server=DESKTOP-T57IBGI\SQLEXPRESS;Database=AuRg;Integrated Security=True";

            using (SqlConnection con = new SqlConnection(connectionString))
            {
                try
                {
                    con.Open();

                    string query = "INSERT INTO [dbo].[user] (gmail, user_password) VALUES (@gmail, @user_password)";
                    SqlCommand cmd = new SqlCommand(query, con);
                    cmd.Parameters.AddWithValue("@gmail", tb1.Text);
                    cmd.Parameters.AddWithValue("@user_password", tb2.Password);

                    int result = cmd.ExecuteNonQuery();

                    if (result > 0)
                    {
                        MessageBox.Show("Регистрация прошла успешно");
                        Login secondWindow = new Login();
                        secondWindow.Show();
                        this.Close();
                    }
                    else
                    {
                        MessageBox.Show("Ошибка регистрации");
                    }
                }
                catch (SqlException ex)
                {
                    MessageBox.Show("Ошибка при работе с базой данных: " + ex.Message);
                }
            }

        }
        private void TransparentButton_Click1(object sender, RoutedEventArgs e)
        {
            Login secondWindow = new Login();
            secondWindow.Show();
            this.Close();
        }

    }

}

Login

using Google.Apis.Auth.OAuth2;
using Google.Apis.Util.Store;
using System;
using System.Data.Entity;
using System.Data.SqlClient;
using System.IO;
using System.Threading;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Input;

namespace YutaPhe
{
    public partial class Login : Window
    {

        public Login()
        {
            InitializeComponent();
        }

        private void Window_MouseDown1(object sender, MouseButtonEventArgs e)
        {
            if (e.ChangedButton == MouseButton.Left)
            {
                this.DragMove();
            }
        }

        private void MinButton_MouseDown(object sender, MouseButtonEventArgs e)
        {
            this.WindowState = WindowState.Minimized;
        }

        private void ExitButton_MouseDown(object sender, MouseButtonEventArgs e)
        {
            this.Close();
        }

        private void Tb2_PasswordChanged(object sender, RoutedEventArgs e)
        {
            // Скрываем текст-подсказку, когда пользователь начинает вводить пароль
            tbPlaceholder.Visibility = string.IsNullOrEmpty(tb2.Password) ? Visibility.Visible : Visibility.Collapsed;
        }


        private void TbPasswordVisible_TextChanged(object sender, TextChangedEventArgs e)
        {

        }

        private void tbPasswordVisible_TextChanged_1(object sender, TextChangedEventArgs e)
        {

        }

        private void tb2_PasswordChanged(object sender, RoutedEventArgs e)
        {
            // Обновляем текст в TextBox при изменении пароля
            if (!showPasswordCheckBox.IsChecked.GetValueOrDefault())
            {
                tbPasswordVisible.Text = tb2.Password; // Сохраняем пароль для отображения

            }
        }

        private void ShowPasswordCheckBox_Checked(object sender, RoutedEventArgs e)
        {
            // Скрываем PasswordBox и показываем TextBox
            tbPasswordVisible.Text = tb2.Password; // Копируем пароль
            tb2.Visibility = Visibility.Collapsed;
            tbPasswordVisible.Visibility = Visibility.Visible;
        }

        private void ShowPasswordCheckBox_Unchecked(object sender, RoutedEventArgs e)
        {
            // Скрываем TextBox и показываем PasswordBox
            tb2.Password = tbPasswordVisible.Text; // Копируем текст обратно в PasswordBox
            tbPasswordVisible.Visibility = Visibility.Collapsed;
            tb2.Visibility = Visibility.Visible;
        }


        private void TransparentButton_Click(object sender, RoutedEventArgs e)
        {
            MainWindow secondWindow = new MainWindow();
            secondWindow.Show();
            this.Close();
        }

        private void TransparentButton_Click1(object sender, RoutedEventArgs e)
        {
            string connectionString = @"Server=DESKTOP-T57IBGI\SQLEXPRESS;Database=AuRg;Integrated Security=True";

            using (SqlConnection con = new SqlConnection(connectionString))
            {
                try
                {
                    con.Open();
                    string query = "SELECT COUNT(*) FROM [dbo].[user] WHERE gmail=@gmail AND user_password=@user_password";
                    SqlCommand cmd = new SqlCommand(query, con);
                    cmd.Parameters.AddWithValue("@gmail", tb1.Text);
                    cmd.Parameters.AddWithValue("@user_password", tb2.Password);

                    int count = (int)cmd.ExecuteScalar();

                    if (count > 0)
                    {
                        MessageBox.Show("Вход выполнен успешно!");
                        Main secondWindow = new Main();
                        secondWindow.Show();
                        this.Close();
                    }
                    else
                    {
                        MessageBox.Show("Неправильный логин или пароль");
                    }
                }
                catch (SqlException ex)
                {
                    MessageBox.Show("Ошибка при работе с базой данных: " + ex.Message);
                }
            }
        }

        private static string[] Scopes = { "https://www.googleapis.com/auth/userinfo.profile" };


        private async void GoogleLoginButton_Click(object sender, RoutedEventArgs e)
        {
            UserCredential credential;

            // Загрузка файла credentials.json
            using (var stream = new FileStream("client_apps.googleusercontent.com.json", FileMode.Open, FileAccess.Read))
            {
                // Путь для хранения токена
                var credPath = "token.json";
                credential = await GoogleWebAuthorizationBroker.AuthorizeAsync(
                    GoogleClientSecrets.Load(stream).Secrets,
                    Scopes,
                    "user",
                    CancellationToken.None,
                    new FileDataStore(credPath, true));
                {
                }

                // Успешный вход
                MessageBox.Show($"Добро пожаловать, {credential.UserId}");
                Main secondWindow = new Main();
                secondWindow.Show();
                this.Close();

            }
        }

    }
}

Main

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Shapes;
using System.Net.Http;

namespace YutaPhe
{
    /// <summary>
    /// Логика взаимодействия для Main.xaml
    /// </summary>
    public partial class Main : Window
    {
        public Main()
        {
            InitializeComponent();
            LoadCountryAsync();
        }

        private async void LoadCountryAsync()
        {
            string country = await GetCountryAsync();
            CountryTextBlock1.Text = $"Саратовская облась.Энгельс: {country}";

        }

        private static async Task<string> GetCountryAsync()
        {
            using (HttpClient client = new HttpClient())
            {
                try
                {
                    // Используйте бесплатный API для получения информации о стране
                    string url = "https://ipapi.co/country_name/";
                    string country = await client.GetStringAsync(url);
                    return country;
                }
                catch (Exception ex)
                {
                    // Обработка ошибок
                    return "Не удалось определить страну.";
                }
            }
        }
        private void Window_MouseDown(object sender, MouseButtonEventArgs e)
        {
            if (e.ChangedButton == MouseButton.Left)
            {
                this.DragMove();
            }
        }

        private void ExitButton_MouseDown(object sender, MouseButtonEventArgs e)
        {
            this.Close();
        }

        private void MinButton_MouseDown(object sender, MouseButtonEventArgs e)
        {
            this.WindowState = WindowState.Minimized;
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
           
        }
    }
}
