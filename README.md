# Extracting-and-Visualizing-Stock-Data
!pip install yfinance
!pip install bs4
!pip install nbformat
!pip install matplotlib
import warnings
# Ignore all warnings
warnings.filterwarnings("ignore", category=FutureWarning)
# Define Graphing Function
# The make_graph function has been modified to use Matplotlib for static graphs. Earlier, it used Plotly to generate interactive dashboards, which caused issues when uploading the notebook in the MARK assignment submission.

import matplotlib.pyplot as plt

def make_graph(stock_data, revenue_data, stock):
    stock_data_specific = stock_data[stock_data.Date <= '2021-06-14']
    revenue_data_specific = revenue_data[revenue_data.Date <= '2021-04-30']

    fig, axes = plt.subplots(2, 1, figsize=(12, 8), sharex=True)

    # Stock price
    axes[0].plot(pd.to_datetime(stock_data_specific.Date), stock_data_specific.Close.astype("float"), label="Share Price", color="blue")
    axes[0].set_ylabel("Price ($US)")
    axes[0].set_title(f"{stock} - Historical Share Price")

    # Revenue
    axes[1].plot(pd.to_datetime(revenue_data_specific.Date), revenue_data_specific.Revenue.astype("float"), label="Revenue", color="green")
    axes[1].set_ylabel("Revenue ($US Millions)")
    axes[1].set_xlabel("Date")
    axes[1].set_title(f"{stock} - Historical Revenue")

    plt.tight_layout()
    plt.show()
    # Use yfinance to Extract Stock Data
    Tesla= yf.Ticker("TSLA")
    tesla_data=Tesla.history(period="max")
    tesla_data.reset_index(inplace=True)
    tesla_data.head()
    # Use Webscraping to Extract Tesla Revenue Data
    url="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"
html_data = requests.get(url).text
soup=BeautifulSoup(html_data,'html.parser')
!pip install lxml
tesla_revenue = pd.read_html(url)[1]
tesla_revenue.columns = ['Date','Revenue']
tesla_revenue["Revenue"] = tesla_revenue['Revenue'].str.replace(',|\$',"",regex=True)

tesla_revenue.dropna(inplace=True)

tesla_revenue = tesla_revenue[tesla_revenue['Revenue'] != ""]
# tesla_revenue = tesla_revenue.reset_index()
tesla_revenue.tail()
# Use yfinance to Extract Stock Data
GameStop = yf.Ticker("GME")
gme_data=GameStop.history(period="max")
gme_data.reset_index(inplace=True)
gme_data.head()
# Use Webscraping to Extract GME Revenue Data
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html"
html_data_2= requests.get(url).text
Soup = BeautifulSoup(html_data_2,'html.parser')
gme_revenue = pd.read_html(url)[1]
gme_revenue.columns = ['Date','Revenue']
gme_revenue["Revenue"] = gme_revenue["Revenue"].replace(
    {r"\$": "", ",": ""}, regex=True)
gme_revenue.dropna(inplace=True)
gme_revenue= gme_revenue[gme_revenue['Revenue'] != ""]
print(gme_revenue.tail())
make_graph(tesla_data, tesla_revenue, 'Tesla')
make_graph(gme_data, gme_revenue, 'GameStop')

