
 "cells": [
  {
   "cell_type": "raw",
   "id": "dbd55d3f-12d1-4a7b-9db8-a1a7df3a533b",
   "metadata": {
    "tags": []
   },
   "source": [
    "!pip install yfinance\n",
    "!pip install bs4"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "id": "c32b91fb-5c93-42e8-a7cb-4c025c513746",
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "import yfinance as yf\n",
    "import pandas as pd\n",
    "import requests\n",
    "from bs4 import BeautifulSoup\n",
    "import plotly.graph_objects as go\n",
    "from plotly.subplots import make_subplots"
   ]
  },
  {
   "cell_type": "raw",
   "id": "a1a630cd-7e2c-4f59-a7b6-acfdb240d85e",
   "metadata": {
    "tags": []
   },
   "source": [
    "Tesla = yf.Ticker(\"TSLA\")"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "419f851b-3d33-4c77-b3cd-2dc1cba590c5",
   "metadata": {
    "tags": []
   },
   "source": [
    "Tesla_data = Tesla.history(period=\"max\")"
   ]
  },
  {
   "cell_type": "raw",
   "id": "28822b51-2c91-4015-821d-1dc6656f8189",
   "metadata": {
    "tags": []
   },
   "source": [
    "Tesla_data.reset_index(inplace=True)\n",
    "Tesla_data.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "id": "68c279f4-02fa-4c34-8e24-0e967a0389b2",
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "url = \" https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm\"\n",
    "html_data = requests.get(url).text"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "id": "784b820b-1318-4134-a8f6-0af50cf0d08d",
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "soup = BeautifulSoup(html_data,\"html5lib\")"
   ]
  },
  {
   "cell_type": "raw",
   "id": "253c83f0-39ea-41ba-8269-92425d55dad4",
   "metadata": {
    "tags": []
   },
   "source": [
    "tesla_revenue = pd.DataFrame(columns=['Date', 'Revenue'])\n",
    "\n",
    "for table in soup.find_all('table'):\n",
    "\n",
    "    if ('Tesla Quarterly Revenue' in table.find('th').text):\n",
    "        rows = table.find_all('tr')\n",
    "        \n",
    "        for row in rows:\n",
    "            col = row.find_all('td')\n",
    "            \n",
    "            if col != []:\n",
    "                date = col[0].text\n",
    "                revenue = col[1].text.replace(',','').replace('$','')\n",
    "\n",
    "                tesla_revenue = tesla_revenue.append({\"Date\":date, \"Revenue\":revenue}, ignore_index=True)"
   ]
  },
  {
   "cell_type": "raw",
   "id": "d4fdd04a-a171-4d59-aa11-acf9e8d943d7",
   "metadata": {
    "tags": []
   },
   "source": [
    "tesla_revenue[\"Revenue\"] = tesla_revenue['Revenue'].str.replace(',|\\$',\"\")"
   ]
  },
  {
   "cell_type": "raw",
   "id": "95212469-d459-44c5-a843-bb08e65858df",
   "metadata": {
    "tags": []
   },
   "source": [
    "tesla_revenue.dropna(inplace=True)\n",
    "\n",
    "tesla_revenue = tesla_revenue[tesla_revenue['Revenue'] != \"\"]"
   ]
  },
  {
   "cell_type": "raw",
   "id": "4624fd8c-219d-4a23-9c04-05b79b1bf402",
   "metadata": {
    "tags": []
   },
   "source": [
    "tesla_revenue.tail()"
   ]
  },
  {
   "cell_type": "raw",
   "id": "4b748617-ca45-4803-8698-c4aeed075752",
   "metadata": {
    "tags": []
   },
   "source": [
    "GameStop = yf.Ticker(\"GME\")"
   ]
  },
  {
   "cell_type": "raw",
   "id": "e50244f0-35f2-472e-a098-d2dc7d0740ed",
   "metadata": {
    "tags": []
   },
   "source": [
    "gme_data = GameStop.history(period=\"max\")"
   ]
  },
  {
   "cell_type": "raw",
   "id": "7655f700-e579-45a4-9311-7814d2c079fd",
   "metadata": {
    "tags": []
   },
   "source": [
    "gme_data.reset_index(inplace=True)\n",
    "gme_data.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "7d71ddda-fe1b-4380-ae80-42086ba44f0e",
   "metadata": {
    "tags": []
   },
   "outputs": [],
   "source": [
    "url = \"https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html\"\n",
    "\n",
    "html_data  = requests.get(url).text"
   ]
  },
  {
   "cell_type": "raw",
   "id": "97d73b29-ed0e-46a1-9e3e-4c96d5a2f053",
   "metadata": {
    "tags": []
   },
   "source": [
    "soup = BeautifulSoup(html_data,\"html5lib\")"
   ]
  },
  {
   "cell_type": "raw",
   "id": "ce01518c-bfa4-43cf-8c22-3a92dd88e38c",
   "metadata": {
    "tags": []
   },
   "source": [
    "gme_revenue = pd.DataFrame(columns=['Date', 'Revenue'])\n",
    "\n",
    "for table in soup.find_all('table'):\n",
    "\n",
    "    if ('GameStop Quarterly Revenue' in table.find('th').text):\n",
    "        rows = table.find_all('tr')\n",
    "        \n",
    "        for row in rows:\n",
    "            col = row.find_all('td')\n",
    "            \n",
    "            if col != []:\n",
    "                date = col[0].text\n",
    "                revenue = col[1].text.replace(',','').replace('$','')\n",
    "\n",
    "                gme_revenue = gme_revenue.append({\"Date\":date, \"Revenue\":revenue}, ignore_index=True)"
   ]
  },
  {
   "cell_type": "raw",
   "id": "89fe133a-dd06-4954-8d3c-b5ac15a0c49c",
   "metadata": {
    "tags": []
   },
   "source": [
    "gme_revenue.tail()\n"
   ]
  },
  {
   "cell_type": "raw",
   "id": "c93fd6fd-f234-4832-858a-63f4e5ef61c6",
   "metadata": {
    "tags": []
   },
   "source": [
    "def make_graph(stock_data, revenue_data, stock):\n",
    "    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, subplot_titles=(\"Historical Share Price\", \"Historical Revenue\"), vertical_spacing = .3)\n",
    "    stock_data_specific = stock_data[stock_data.Date <= '2021--06-14']\n",
    "    revenue_data_specific = revenue_data[revenue_data.Date <= '2021-04-30']\n",
    "    fig.add_trace(go.Scatter(x=pd.to_datetime(stock_data_specific.Date, infer_datetime_format=True), y=stock_data_specific.Close.astype(\"float\"), name=\"Share Price\"), row=1, col=1)\n",
    "    fig.add_trace(go.Scatter(x=pd.to_datetime(revenue_data_specific.Date, infer_datetime_format=True), y=revenue_data_specific.Revenue.astype(\"float\"), name=\"Revenue\"), row=2, col=1)\n",
    "    fig.update_xaxes(title_text=\"Date\", row=1, col=1)\n",
    "    fig.update_xaxes(title_text=\"Date\", row=2, col=1)\n",
    "    fig.update_yaxes(title_text=\"Price ($US)\", row=1, col=1)\n",
    "    fig.update_yaxes(title_text=\"Revenue ($US Millions)\", row=2, col=1)\n",
    "    fig.update_layout(showlegend=False,\n",
    "    height=900,\n",
    "    title=stock,\n",
    "    xaxis_rangeslider_visible=True)\n",
    "    fig.show()"
   ]
  },
  {
   "cell_type": "raw",
   "id": "ab559cb0-614e-4ed5-9f5b-d3c2f77f1d70",
   "metadata": {
    "tags": []
   },
   "source": [
    "make_graph(Tesla_data, tesla_revenue, \"Tesla\")"
   ]
  },
  {
   "cell_type": "raw",
   "id": "cb7c861d-ac80-4e10-9472-f7827ba3605f",
   "metadata": {
    "tags": []
   },
   "source": [
    "make_graph(gme_data, gme_revenue, 'GameStop')"
   ]
  },
  {
   "cell_type": "raw",
   "id": "30946850-9d9c-4d1a-be1c-1a0064263b54",
   "metadata": {},
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python",
   "language": "python",
   "name": "conda-env-python-py"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.12"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
