VERISHIELD SM300 - SHIPPING ORDER CANCELLATION UTILITY (HTA)
============================================================

WHAT IT IS
A single HTML Application (SO_Cancel_Utility.hta). Windows runs it directly
with its built-in mshta.exe. No Python, no installation.

It cancels a shipping order by calling curecloud.sp_cancelShippingOrder_custom
(which calls update_root_cancel_so) through the mysql.exe command-line client.

REQUIREMENTS
- Windows 7 / 10 / 11 or Windows Server.
- mysql.exe. It is already installed with MySQL Server on the SM300 server,
  typically C:\Program Files\MySQL\MySQL Server 8.0\bin\mysql.exe.
  The utility finds it by itself. To run from a PC without MySQL, copy
  mysql.exe and the .dll files next to it in that "bin" folder
  (e.g. libcrypto-3-x64.dll, libssl-3-x64.dll) into this utility's folder.
- Both stored procedures must exist in the curecloud database.
  The utility checks this when it connects.

HOW TO USE
1. Copy SO_Cancel_Utility.hta to a folder you can write to (e.g. Desktop\SO_Cancel).
2. If it came from email or a download: right-click > Properties > tick "Unblock" > OK.
3. Double-click SO_Cancel_Utility.hta.
4. Enter MySQL hostname/IP, port and root password. Check the mysql.exe path. Click Connect.
5. Enter Shipping Order ID and User ID. Click Check order.
6. Review the details, type the SO ID again, click Cancel shipping order.

RULES (same as the stored procedure)
- Only orders in status 3 or 14 can be cancelled. Others are refused.
- The User ID must pass the SM300 access check (f_access_control).
- On success the SO status becomes 4 (Cancelled).

FILES IT CREATES (next to the .hta)
- SO_Cancel_Log.csv       every cancellation attempt: time, Windows user, PC,
                          MySQL host, SO ID, User ID, result and messages.
- SO_Cancel_Settings.txt  last hostname, port and mysql.exe path.
The root password is never saved. For each call it is written to a temporary
file in your Windows TEMP folder and deleted as soon as mysql.exe finishes,
so it never appears on a command line.

TROUBLESHOOTING
- "Access denied for root": wrong password, or root is not allowed from this PC.
- "Cannot reach MySQL": check IP, port 3306, firewall and the MySQL service.
- "authentication plugin mismatch": use the mysql.exe from the same MySQL
  version as the server.
- Passwords containing both ' and " cannot be used (MySQL client limitation).
