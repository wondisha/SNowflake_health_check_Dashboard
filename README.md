Snowflake Health Check Dashboard
This Streamlit application provides a comprehensive dashboard for monitoring the health and performance of your Snowflake account. It leverages Snowflake's ACCOUNT_USAGE views to present insights into query performance, warehouse utilization, storage trends, task automation, and login history.

Features
Query Performance Insights:

Identifies long-running queries and their resource consumption (bytes scanned, partitions).

Leverages Snowflake's native QUERY_INSIGHTS to highlight issues like disk spillage, unselective filters, and poor pruning.

Provides conceptual integration for AI-powered (LLM) query optimization suggestions.

Analyzes failed queries to help troubleshoot common errors.

Resource Utilization Monitoring:

Tracks warehouse credit usage over time, broken down by compute and cloud services.

Identifies top warehouses and users by credit consumption and bytes scanned.

Storage Usage Trends:

Monitors database and Failsafe storage consumption over time in GB.

Shows current storage usage by individual databases.

Task & Automation History:

Displays the execution history of Snowflake tasks, including their state, duration, and any errors.

Summarizes task statuses and highlights failed tasks.

Login & Security Monitoring:

Reviews recent login attempts, including user, IP, and success status.

Highlights failed login attempts for security auditing.

Interactive Filters: Use sidebar sliders to adjust the time ranges for various data sections.

Overview Tab: Get a quick summary of key metrics like total queries, failed queries, average execution time, and overall credit usage.

Caching: Data fetching functions are cached for 10 minutes (ttl=600), ensuring quick load times for repeated access while keeping data reasonably fresh.

How it Works
This application is designed to be deployed as a Streamlit in Snowflake Native App or a Streamlit in Snowflake application.

Snowflake Connection: The app automatically connects to your current Snowflake session using st.connection("snowflake"). This means you don't need to configure explicit connection details (like username/password) within the app's code or secrets.toml for the primary Snowflake connection.

Data Retrieval: It queries various views within the SNOWFLAKE.ACCOUNT_USAGE schema (QUERY_INSIGHTS, QUERY_HISTORY, WAREHOUSE_METERING_HISTORY, LOGIN_HISTORY, DATABASE_STORAGE_USAGE_HISTORY, TASK_HISTORY) to gather operational data.

Data Processing: Fetched data is loaded into Pandas DataFrames, and relevant columns are converted to appropriate data types.

Streamlit UI: Streamlit is used to create an interactive web interface with multiple tabs for logical organization of the monitoring data.

