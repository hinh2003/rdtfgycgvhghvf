import java.sql.*;

public class SalesAnalysis {
    public static void main(String[] args) {
        String driverName = "org.apache.hive.jdbc.HiveDriver";
        String url = "jdbc:hive2://localhost:10000/default";
        String username = "";
        String password = "";

        try {
            Class.forName(driverName);
            Connection conn = DriverManager.getConnection(url, username, password);
            Statement stmt = conn.createStatement();

            // Thực hiện truy vấn Hive để lấy tổng doanh số bán hàng trong mỗi ngày/tháng/năm
            String query = "SELECT SUBSTR(timestamp, 1, 10) AS sale_date, " +
                           "SUM(quantity * unit_price) AS total_sales " +
                           "FROM transactions " +
                           "GROUP BY SUBSTR(timestamp, 1, 10)";
            ResultSet rs = stmt.executeQuery(query);

            // In ra kết quả
            System.out.println("Doanh số bán hàng theo ngày:");
            while (rs.next()) {
                String saleDate = rs.getString("sale_date");
                double totalSales = rs.getDouble("total_sales");
                System.out.println(saleDate + ": " + totalSales);
            }

            rs.close();
            stmt.close();
            conn.close();
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }


   // Thực hiện truy vấn Hive để đếm số lượng giao dịch trong mỗi ngày/tháng/năm
            String query = "SELECT SUBSTR(timestamp, 1, 10) AS transaction_date, " +
                           "COUNT(*) AS transaction_count " +
                           "FROM transactions " +
                           "GROUP BY SUBSTR(timestamp, 1, 10)";
            ResultSet rs = stmt.executeQuery(query);

            // In ra kết quả
            System.out.println("Số lượng giao dịch theo ngày:");
            while (rs.next()) {
                String transactionDate = rs.getString("transaction_date");
                int transactionCount = rs.getInt("transaction_count");
                System.out.println(transactionDate + ": " + transactionCount);
            }












        
    }
}
