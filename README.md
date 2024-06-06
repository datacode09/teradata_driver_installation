### Updating Teradata ODBC Driver to Version 17.20

Follow these steps to backup the existing Teradata ODBC driver, remove it, and install the new driver from a `.tar.gz` package.

#### Step 1: Backup Existing Teradata ODBC Driver

1. **Create a Backup of the Existing Directory:**

    ```sh
    sudo cp -r /opt/teradata/client /opt/teradata/client_backup
    ```

#### Step 2: Remove the Existing Directory

1. **Remove the Existing `/opt/teradata/client` Directory:**

    ```sh
    sudo rm -rf /opt/teradata/client
    ```

#### Step 3: Install the New Tar.gz Package

1. **Extract the `.tar.gz` File:**

    ```sh
    cd /tmp/teradata_odbc_1720
    tar -xzvf teradata_driver.tar.gz
    ```

2. **Locate the Extracted RPM File:**

    ```sh
    ls /tmp/teradata_odbc_1720
    ```

    - You should see the extracted RPM file, such as `teradata-odbc-driver-17.20.xx.rpm`.

3. **Install the RPM Package:**

    ```sh
    sudo rpm -ivh /tmp/teradata_odbc_1720/teradata-odbc-driver-17.20.xx.rpm
    ```

    - Replace `teradata-odbc-driver-17.20.xx.rpm` with the actual RPM filename.

#### Step 4: Update Configuration Files

1. **Update `odbcinst.ini`:**

    ```sh
    sudo nano /etc/odbcinst.ini
    ```

    Add or update the entry for the Teradata driver:

    ```ini
    [Teradata]
    Description=Teradata ODBC Driver 17.20
    Driver=/opt/teradata/client/17.20/ODBC_64/lib/tdataodbc_sb64.so
    ```

2. **Update `odbc.ini`:**

    ```sh
    sudo nano /etc/odbc.ini
    ```

    Ensure the DSN entry is correct:

    ```ini
    [TeradataDSN]
    Description=My Teradata Data Source
    Driver=Teradata
    Database=your_database_name
    ServerName=your_teradata_server
    ```

3. **Update Environment Variables (if necessary):**

    ```sh
    sudo sh -c 'echo "export LD_LIBRARY_PATH=\$LD_LIBRARY_PATH:/opt/teradata/client/17.20/ODBC_64/lib" >> /etc/profile'
    source /etc/profile
    ```

#### Step 5: Verify the Installation

1. **Test the DSN with `isql`:**

    ```sh
    isql -v TeradataDSN your_username your_password
    ```

2. **Test with `pyodbc` in Python:**

    ```python
    import pyodbc

    connection_string = "DSN=TeradataDSN;UID=your_username;PWD=your_password"
    conn = pyodbc.connect(connection_string)

    # Example query
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM your_table")
    for row in cursor:
        print(row)

    # Close the connection
    conn.close()
    ```

By following these steps, you will ensure a clean installation of the new Teradata ODBC driver and properly update your environment and configuration files to use the new driver. If you encounter any issues or need further assistance, please contact the support team.
