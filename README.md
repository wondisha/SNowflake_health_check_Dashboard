# Snowflake Health Check Dashboard

This Streamlit application provides a comprehensive dashboard for monitoring the health and performance of your Snowflake account. It leverages Snowflake's `ACCOUNT_USAGE` views to present insights into query performance, warehouse utilization, storage trends, task automation, and login history.

---

## Features

* **Query Performance Insights:**
    * Identifies long-running queries and their resource consumption (bytes scanned, partitions).
    * Leverages Snowflake's native `QUERY_INSIGHTS` to highlight issues like disk spillage, unselective filters, and poor pruning.
    * Provides conceptual integration for **AI-powered (LLM) query optimization suggestions**.
    * Analyzes failed queries to help troubleshoot common errors.
* **Resource Utilization Monitoring:**
    * Tracks warehouse credit usage over time, broken down by compute and cloud services.
    * Identifies top warehouses and users by credit consumption and bytes scanned.
* **Storage Usage Trends:**
    * Monitors database and Failsafe storage consumption over time in GB.
    * Shows current storage usage by individual databases.
* **Task & Automation History:**
    * Displays the execution history of Snowflake tasks, including their state, duration, and any errors.
    * Summarizes task statuses and highlights failed tasks.
* **Login & Security Monitoring:**
    * Reviews recent login attempts, including user, IP, and success status.
    * Highlights failed login attempts for security auditing.
* **Interactive Filters:** Use sidebar sliders to adjust the time ranges for various data sections.
* **Overview Tab:** Get a quick summary of key metrics like total queries, failed queries, average execution time, and overall credit usage.
* **Caching:** Data fetching functions are cached for 10 minutes (`ttl=600`), ensuring quick load times for repeated access while keeping data reasonably fresh.

---

## How it Works

This application is designed to be deployed as a **Streamlit in Snowflake Native App** or a **Streamlit in Snowflake application**.

1.  **Snowflake Connection:** The app automatically connects to your current Snowflake session using `st.connection("snowflake")`. This means you don't need to configure explicit connection details (like username/password) within the app's code or `secrets.toml` for the primary Snowflake connection.
2.  **Data Retrieval:** It queries various views within the `SNOWFLAKE.ACCOUNT_USAGE` schema (`QUERY_INSIGHTS`, `QUERY_HISTORY`, `WAREHOUSE_METERING_HISTORY`, `LOGIN_HISTORY`, `DATABASE_STORAGE_USAGE_HISTORY`, `TASK_HISTORY`) to gather operational data.
3.  **Data Processing:** Fetched data is loaded into Pandas DataFrames, and relevant columns are converted to appropriate data types.
4.  **Streamlit UI:** Streamlit is used to create an interactive web interface with multiple tabs for logical organization of the monitoring data.

---

## Setup and Deployment (Streamlit in Snowflake)

1.  **Ensure ACCOUNT_USAGE Access:** Your Snowflake role must have `ACCOUNTADMIN` privileges or be granted `IMPORTED PRIVILEGES` on the `SNOWFLAKE` database to access the `ACCOUNT_USAGE` views.
    ```sql
    GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO ROLE YOUR_ROLE;
    ```
    Replace `YOUR_ROLE` with the role your Streamlit app will run under.

2.  **Adjust Role Name:** In the `get_query_history` function, update the placeholder role `'ABC'` to a relevant role in your Snowflake environment to filter queries by users belonging to that role:
    ```python
    # Locate this line in get_query_history function:
    # WHERE role_name = 'ABC' -- <<< IMPORTANT: Adjust 'ABC' to your relevant role name
    ```

3.  **Create a Streamlit App in Snowflake:**
    * Log in to your Snowflake account.
    * Navigate to **Snowflake Native App Studio** (or similar if using an older version of Streamlit in Snowflake).
    * Create a new Streamlit app.
    * Paste the entire Python code into the app editor.
    * Specify a **warehouse** that the app will use to run its queries.

4.  **Run the App:** Save and run the Streamlit application. It will automatically connect to your Snowflake environment and display the dashboard.

---

## Important Considerations

* **Data Latency:** Data in `ACCOUNT_USAGE` views has a latency of up to several hours. This dashboard is suitable for operational monitoring, not real-time critical alerting.
* **Role Permissions:** The accuracy and completeness of the data displayed depend entirely on the permissions of the Snowflake role that executes the Streamlit app.
* **Credit Usage:** Running this app will consume Snowflake credits based on the queries executed by the underlying warehouse.
* **LLM Integration:** The LLM integration section is conceptual. To enable actual AI-powered suggestions, you would need to:
    1.  Obtain an API key from an LLM provider (e.g., Google Gemini, OpenAI).
    2.  Store this API key securely using `st.secrets` in your Streamlit in Snowflake app's environment.
    3.  Implement the actual API call within the `Get LLM Suggestions for this Query` button's logic.
* **Caching (`st.cache_data`):** The app uses `st.cache_data(ttl=600)` to cache query results for 10 minutes. This reduces repeated queries to `ACCOUNT_USAGE` views, improving performance and reducing credit consumption. Data will refresh automatically from Snowflake after the cache expires or if the input parameters to the cached functions change.

---

This dashboard provides a powerful way to gain visibility into your Snowflake usage and identify areas for optimization and security improvement.
