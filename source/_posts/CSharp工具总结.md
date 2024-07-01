---
title: CSharp工具总结
date: 2022-12-16T12:12:57.000Z
updated: '2024-07-01 21:57:09'
categories:
  - C#
tags:
  - C#
  - Tools
---
C# DB相关代码
<!-- more -->

## DB连接
> 文件连接已集成到工具类，里参见TestDB()方法 

```csharp
//编写数据库连接串
string connStr = "Data source=.;Initial Catalog=test;User ID=sa;Password=pwdpwd";
//创建SqlConnection的实例
SqlConnection conn = null;
try
{
    conn = new SqlConnection(connStr);
    //打开数据库连接
    conn.Open();
    MessageBox.Show("数据库连接成功！");
}
catch(Exception ex)
{
    MessageBox.Show("数据库连接失败！" + ex.Message);
}
finally
{
    if (conn != null)
    {
        //关闭数据库连接
        conn.Close();
    }
}
```

## DB文件
```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <add key="connStrId" value="name"/>
    <add key="connStrPassword" value="密码"/>
    <add key="connStrHost" value="ip地址"/>
    <add key="connStrPort" value="端口号"/>
    <add key="connStrService" value="PDBORCL"/> //连接服务 可插拔数据库
  </appSettings>
</configuration>
```

## DB操作工具类
```csharp
using System;
using System.Data;
using Oracle.ManagedDataAccess.Client;
using System.Configuration;
using System.Xml;

/*缺个namespace 后补*/
/// <summary>
/// DB操作方法
/// </summary>
class DbHelper
    {
        public static Result TestDB()
        {
            Result rs = new Result();
            OracleConnection connection = DbHelper.GetConnection();

            try
            {
                connection.Open();
            }
            catch (OracleException ex)
            {
                rs.ErrCode = 1;
                rs.ErrMsg = ex.Message.ToString();
            }
            finally
            {
                if (connection != null)
                {

                    connection.Close();
                }

            }
            return rs;
        }
        /// <summary>
        /// 通过配置文件连接数据库
        /// </summary>
        /// <returns></returns>
        public static  OracleConnection GetConnection()
        {
            try
            {
                string connStr = "User Id=" + ConfigurationManager.AppSettings["connStrId"].ToString() + ";Password=" + ConfigurationManager.AppSettings["connStrPassword"].ToString() + ";Connection Timeout=4" + ";Data Source=(DESCRIPTION=(ADDRESS_LIST=(ADDRESS=(PROTOCOL=TCP)(HOST=" +
                    ConfigurationManager.AppSettings["connStrHost"].ToString() + ")(PORT=" + ConfigurationManager.AppSettings["connStrPort"].ToString() + ")))(CONNECT_DATA=(SERVICE_NAME=" + ConfigurationManager.AppSettings["connStrService"].ToString() +
                    ")))";
                OracleConnection conn = new OracleConnection(connStr);
                return conn;
            }
            catch (OracleException ex)
            {
                throw new Exception(ex.Message);
            }
            catch (NullReferenceException ex)
            {
                throw new Exception("Config文件未发现。执行确认。(" + ex.Message + ")");
            }
        }
        /// <summary>
        /// SQL无参查询 返回set结果集
        /// </summary>
        /// <param name="SQLString"></param>
        /// <returns></returns>
        public static DataSet Query(string SQLString)
        {
            OracleConnection connection = GetConnection();

            DataSet ds = new DataSet();
            try
            {
                connection.Open();
                OracleDataAdapter command = new OracleDataAdapter(SQLString, connection);
                command.Fill(ds, "ds");
            }
            catch (OracleException ex)
            {
                throw new Exception(ex.Message);
            }
            finally
            {
                if (connection != null)
                {

                    connection.Close();
                }
            }
            return ds;

        }
        /// <summary>
        /// 无参查询返回 DataTable类型
        /// </summary>
        /// <param name="SQLString"></param>
        /// <returns></returns>
        public static DataTable Query_DT(string SQLString)
        {
            OracleConnection connection = GetConnection();

            DataTable table = new DataTable();
            try
            {
                connection.Open();
                OracleDataAdapter command = new OracleDataAdapter(SQLString, connection);
                table.Locale = System.Globalization.CultureInfo.InvariantCulture;
                command.Fill(table);
            }
            catch (OracleException ex)
            {
                throw new Exception(ex.Message);
            }
            finally
            {
                if (connection != null)
                {

                    connection.Close();
                }
            }
            return table;
        }

        /// <summary>
        /// 有参查询 返回 DataTable
        /// </summary>
        /// <param name="SQLString"></param>
        /// <param name="cmdParms"></param>
        /// <returns></returns>
        public static DataTable QueryWithParms_DT(string SQLString, params OracleParameter[] cmdParms)
        {
            OracleConnection connection = GetConnection();

            OracleCommand cmd = new OracleCommand();
            PrepareCommand(cmd, connection, null, SQLString, cmdParms);
            OracleDataAdapter da = new OracleDataAdapter(cmd);

            DataTable table = new DataTable();
            try
            {
                table.Locale = System.Globalization.CultureInfo.InvariantCulture;
                da.Fill(table);
                cmd.Parameters.Clear();
            }
            catch (OracleException ex)
            {
                throw new Exception(ex.Message);
            }
            finally
            {
                if (connection != null)
                {

                    connection.Close();
                }
            }
            return table;
        }
        /// <summary>
        /// 有参查询 返回 set
        /// </summary>
        /// <param name="SQLString"></param>
        /// <param name="cmdParms"></param>
        /// <returns></returns>
        public static DataSet Query(string SQLString, params OracleParameter[] cmdParms)
        {
            OracleConnection connection = GetConnection();

            OracleCommand cmd = new OracleCommand();
            PrepareCommand(cmd, connection, null, SQLString, cmdParms);
            OracleDataAdapter da = new OracleDataAdapter(cmd);

            DataSet ds = new DataSet();
            try
            {
                da.Fill(ds, "ds");
                cmd.Parameters.Clear();
            }
            catch (OracleException ex)
            {
                throw new Exception(ex.Message);
            }
            finally
            {
                if (connection != null)
                {

                    connection.Close();
                }
            }
            return ds;
        }
        /// <summary>
        /// 执行参数查询
        /// </summary>
        /// <param name="cmd">数据库执行命令</param>
        /// <param name="conn">数据库连接命令</param>
        /// <param name="trans">事务</param>
        /// <param name="cmdText">查询语句</param>
        /// <param name="cmdParms">查询参数</param>
        private static void PrepareCommand(OracleCommand cmd, OracleConnection conn, OracleTransaction trans, string cmdText, OracleParameter[] cmdParms)
        {
            if (conn.State != ConnectionState.Open)
                conn.Open();
            cmd.Connection = conn;
            cmd.CommandText = cmdText;
            cmd.BindByName = true;
            if (trans != null)
                cmd.Transaction = trans; //指定SqlCommand类的事务
            cmd.CommandType = CommandType.Text;//指定sqlcommand类的CommandText属性的解释类型
            if (cmdParms != null) //如果插入的参数不为空，则foreach循环遍历加入到SqlCommand类的参数属性中
            {
                foreach (OracleParameter parm in cmdParms)
                {
                    if (cmdText.Contains(parm.ParameterName))
                    {
                        cmd.Parameters.Add(parm);
                    }
                }
            }
        }

        /// <summary>
        /// 执行存储过程 返回SqlDataReader ( 注意：调用该方法后，一定要对SqlDataReader进行Close )
        /// </summary>
        /// <param name="storedProcName">存储过程名</param>
        /// <param name="parameters">存储过程参数</param>
        /// <returns>OracleDataReader</returns>
        public static OracleDataReader RunProcedure(string storedProcName, IDataParameter[] parameters)
        {
            OracleConnection connection = GetConnection();
            OracleDataReader returnReader;
            connection.Open();
            OracleCommand command = BuildQueryCommand(connection, storedProcName, parameters);
            command.CommandType = CommandType.StoredProcedure;
            returnReader = command.ExecuteReader();
            command.Parameters.Clear();

            if (connection != null)
            {

                connection.Close();
            }
            return returnReader;
        }

        /// <summary>
        /// 构建 OracleCommand 对象(用来返回一个结果集，而不是一个整数值)
        /// </summary>
        /// <param name="connection">数据库连接</param>
        /// <param name="storedProcName">存储过程名</param>
        /// <param name="parameters">存储过程参数</param>
        /// <returns>OracleCommand</returns>
        private static OracleCommand BuildQueryCommand(OracleConnection connection, string storedProcName, IDataParameter[] parameters)
        {
            OracleCommand command = new OracleCommand(storedProcName, connection);
            command.CommandType = CommandType.StoredProcedure;
            command.BindByName = true;
            foreach (OracleParameter parameter in parameters)
            {
                command.Parameters.Add(parameter);
            }
            return command;
        }

        /// <summary>
        /// 执行SQL语句，返回影响的记录数
        /// </summary>
        /// <param name="SQLString">SQL语句</param>
        /// <returns>影响的记录数</returns>
        public static int ExecuteSql(string SQLString)
        {
            OracleConnection connection = GetConnection();
            OracleCommand cmd = new OracleCommand(SQLString, connection);

            try
            {
                connection.Open();
                int rows = cmd.ExecuteNonQuery();
                return rows;
            }
            catch (OracleException E)
            {
                connection.Close();
                throw new Exception(E.Message);
            }
            finally
            {
                if (connection != null)
                {

                    connection.Close();
                }
            }
        }
        /// <summary>
        /// 执行SQL语句，返回影响的记录数
        /// </summary>
        /// <param name="SQLString">SQL语句<</param>
        /// <param name="cmdParms">SQL参数</param>
        /// <returns></returns>
        public static int ExecuteSqlWithParam(string SQLString, params OracleParameter[] cmdParms)
        {
            OracleConnection connection = GetConnection();
            OracleCommand cmd = new OracleCommand();
            PrepareCommand(cmd, connection, null, SQLString, cmdParms);

            try
            {
                int rows = cmd.ExecuteNonQuery();
                cmd.Parameters.Clear();
                return rows;
            }
            catch (OracleException E)
            {
                connection.Close();
                throw new Exception(E.Message);
            }
            finally
            {
                if (connection != null)
                {

                    connection.Close();
                }
            }
        }
        /// <summary>
        /// 更改数据库配置文件
        /// </summary>
        /// <param name="keyName"></param>
        /// <param name="keyValue"></param>
        public static void UpdateAppSettings(string keyName, string keyValue)
        {
            XmlDocument XmlDoc = new XmlDocument();
            XmlDoc.Load(AppDomain.CurrentDomain.SetupInformation.ConfigurationFile);

            foreach (XmlElement xElement in XmlDoc.DocumentElement)
            {
                if ("appSettings".Equals(xElement.Name))
                {
                    foreach (XmlNode xNode in xElement.ChildNodes)
                    {
                        if (keyName.Equals(xNode.Attributes[0].Value))
                        {
                            xNode.Attributes[1].Value = keyValue;
                        }
                    }
                }
            }
            XmlDoc.Save(AppDomain.CurrentDomain.SetupInformation.ConfigurationFile);
        }

    }
```

## 性能改善-线程
> 简简单单，线程池一步搞定 

```csharp
//定义委托
public delegate void WaitCallback(object state);
//该方法 在包 System.Threading里

using System.Threading
class Program
    {
        static void Main(string[] args)
        {
            ThreadPool.QueueUserWorkItem(ThreadRun,new Object());//后面的是传递参数，可以省略
        }
        // param 表示要传递的参数信息
        private static void ThreadRun(Object param)
        {
            //执行内容
            Console.WriteLine("任务已被执行1");
        }
    }

```
