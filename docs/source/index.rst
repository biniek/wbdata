.. wbdata documentation master file, created by
   sphinx-quickstart on Sat Sep  1 20:11:30 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to wbdata's documentation!
==================================
What is wbdata?
---------------
Wbdata is a simple python interface to find and request information from the
World Bank's various databases, either as a dictionary containing full metadata
or as a `pandas <http://pandas.pydata.org>`_ DataFrame.  Currently, wbdata
wraps most of the `World Bank API
<http://data.worldbank.org/developers/api-overview>`_, and also adds some
convenience functions for searching and retrieving information.

Wbdata was designed to be used either in a script or in a shell.  In a shell,
wbdata assumes that the user will use most functions to look up the codes
necessary to retrieve the information he wants. To this end, the default in
shell mode for most functions is to simply print the id and human-readable name
of each item in question.  In a script, the default is to return the entire
response from the World Bank converted to python objects.

All the functions that you need to get started are in the :doc:`wbdata` module.

Finally, it should be pointed out that wbdata is in the "release early" portion
of the "release early, release often" cycle, and the current test suite is
pretty perfunctory.  You won't end up with the wrong data, but any
irregularities I haven't specifically encountered in the World Bank database
have not been dealt with.

Full Package Documentation
--------------------------
.. toctree::
    :maxdepth: 2

    wbdata
    api
    fetcher


Installation
------------
Wbdata is available on `PyPi <https://pypi.python.org/pypi/wbdata>`_ which
means you can download it from there or install using easy_install or pip:

    easy_install wbdata

    pip install wbdata

You can also download or get the source from `GitHub
<http://github.com/OliverSherouse/wbdata>`_.


Suggested Citation
------------------
If you use wbdata in a paper, it would be great to cite it.  It lets me know to
keep working on it, and it lets other researchers know how to make their lives
easier.  Suggested citation would be something like:

.. epigraph::

    Sherouse, Oliver (2014). Wbdata. Arlington, VA.  Available from
    http://github.com/OliverSherouse/wbdata.


A Typical User Session
----------------------
Let's say we want to find some data for the ease of doing business in some
well-off countries.  I might start off by seeing what sources are available and
look promising:

    >>> wbdata.get_source()
    11	Africa Development Indicators
    31	Country Policy and Institutional Assessment (CPIA) 
    26	Corporate Scorecard
    1 	Doing Business
    30	Exporter Dynamics Database: Country-Year
    12	Education Statistics
    13	Enterprise Surveys
    28	Global Findex ( Global Financial Inclusion database)
    33	G20 Basic Set of Financial Inclusion Indicators
    14	Gender Statistics
    15	Global Economic Monitor
    27	GEP Economic Prospects
    32	Global Financial Development
    21	Global Economic Monitor (GEM) Commodities
    29	Global Social Protection
    16	Health Nutrition and Population Statistics
    18	International Development Association - Results Measurement System
    6 	International Debt Statistics
    25	Jobs for Knowledge Platform
    19	Millennium Development Goals
    24	Povstats
    20	Public Sector Debt
    23	Quarterly External Debt Statistics (QEDS) - General Data Dissemination System (GDDS)
    22	Quarterly External Debt Statistics (QEDS) - Special Data Dissemination Standard (SDDS)
    2 	World Development Indicators
    3 	Worldwide Governance Indicators

Well, that "Doing Business"---source 1---looks like a winner.  Let's see what
we've got available to us there.

    >>> wbdata.get_indicator(source=1)
    IC.BUS.EASE.XQ   	Ease of doing business index (1=most business-friendly regulations)
    IC.CRD.INFO.XQ   	Credit depth of information index (0=low to 6=high)
    IC.CRD.PRVT.ZS   	Private credit bureau coverage (% of adults)
    IC.CRD.PUBL.ZS   	Public credit registry coverage (% of adults)
    IC.DCP.COST      	Cost to build a warehouse (% of income per capita)
    IC.DCP.PROC      	Procedures required to build a warehouse (number)
    IC.DCP.TIME      	Time required to build a warehouse (days)
    IC.EC.COST       	Cost to enforce a contract (% of claim) 
    IC.EC.PROC       	Procedures required to enforce a contract (number)
    IC.EC.TIME       	Time required to enforce a contract (days)
    IC.EXP.COST.EXP  	Trade: Cost to export (US$ per container)
    IC.EXP.COST.IMP  	Trade: Cost to import (US$ per container)
    IC.EXP.DOCS      	Documents to export (number)
    IC.EXP.DOCS.IMP  	Trade: Documents to import (number)
    IC.EXP.TIME.EXP  	Trade: Time to export (day)
    IC.EXP.TIME.IMP  	Trade: Time to import (days)
    IC.GE.COST       	Cost to get electricity(% of income per capita)
    IC.GE.NUM        	Procedures required to connect to electricity (number)
    IC.GE.TIME       	Time required to connect to electricity (days)
    IC.IMP.DOCS      	Documents to import (number)
    IC.ISV.COST      	Resolving insolvency: cost (% of estate)
    IC.ISV.DURS      	Time to resolve insolvency (years)
    IC.ISV.RECRT     	Resolving insolvency: recovery rate (cents on the dollar)
    IC.LGL.CRED.XQ   	Strength of legal rights index (0=weak to 10=strong)
    IC.LIC.NUM       	Procedures required to build a warehouse (number)
    IC.LIC.TIME      	Time required to build a warehouse (days)
    IC.PI.DIR        	Extent of director liability index (0 to 10)
    IC.PI.DISCL      	Extent of disclosure index (0 to 10)
    IC.PI.INV        	Strength of investor protection index (0 to 10)
    IC.PI.SHAR       	Ease of shareholder suits index (0 to 10) 
    IC.REG.CAP       	Minimum paid-in capital required to start a business (% of income per capita)
    IC.REG.COST      	Cost to start a business (% of income per capita)
    IC.REG.DURS      	Time required to start a business (days)
    IC.REG.PROC      	Start-up procedures to register a business (number)
    IC.RP.COST       	Cost to register property (% of property value)
    IC.RP.PROC       	Procedures required to register property (number)
    IC.RP.TIME       	Time required to register property (days)
    IC.TAX.DURS      	Time to prepare and pay taxes (hours)
    IC.TAX.PAYM      	Tax payments (number)
    IC.TAX.TOTL.CP.ZS	Total tax rate (% of commercial profits)

Alrighty.  There's a lot there.  But let's say I'm in the early stages of
developing a question and go for the most general measure, which is the first
one, the "Ease of Doing Business Index", which has the id "IC.BUS.EASE.XQ".

Now remember, we're only interested in high-income countries right now, because
we're elitist.  So let's use one of the convenience search functions to figure
out the code for the United States so we don't have to wait for data from a
bunch of other countries:

    >>> wbdata.search_countries("united")
    ARE	United Arab Emirates
    GBR	United Kingdom
    USA	United States

"USA". Very creative.  Thank you, World Bank.  But in any case, let's get our
data:

    >>> wbdata.get_data("IC.BUS.EASE.XQ", country="USA")

And that will return a big long list of dictionaries with all the relevant
data and metadata as organized by the World Bank.  Now let's say we want to
look at the United Kingdom as well ("GBR", see above), and only for the years
2010-2011.  We can actually search using multiple countries and restrict the
dates using datetime objects. Here's what that would look like:

    >>> data_date = (datetime.datetime(2010, 1, 1), datetime.datetime(2011, 1, 1))
    >>> wbdata.get_data("IC.BUS.EASE.XQ", country=("USA", "GBR"), data_date=data_date)

And we get another long list of dictionaries, which we can parse any which way
we please.

So let's get a little bit more econometric. Let's say we want to fetch this
same indicator, but also GDP per capita and for all high-income countries.  Let's find
the other indicator we want using another convenience search function:

    >>> wbdata.search_indicators("gdp per capita")
    GDPPCKD             	GDP per Capita, constant US$, millions
    GDPPCKN             	Real GDP per Capita (real local currency units, various base years)
    NV.AGR.PCAP.KD.ZG   	Real agricultural GDP per capita growth rate (%)
    NY.GDP.PCAP.CD      	GDP per capita (current US$)
    NY.GDP.PCAP.KD      	GDP per capita (constant 2000 US$)
    NY.GDP.PCAP.KD.ZG   	GDP per capita growth (annual %)
    NY.GDP.PCAP.KN      	GDP per capita (constant LCU)
    NY.GDP.PCAP.PP.CD   	GDP per capita, PPP (current international $)
    NY.GDP.PCAP.PP.KD   	GDP per capita, PPP (constant 2005 international $)
    NY.GDP.PCAP.PP.KD.ZG	GDP per capita, PPP annual growth (%)
    SE.XPD.PRIM.PC.ZS   	Expenditure per student, primary (% of GDP per capita)
    SE.XPD.SECO.PC.ZS   	Expenditure per student, secondary (% of GDP per capita)
    SE.XPD.TERT.PC.ZS   	Expenditure per student, tertiary (% of GDP per capita)

Like good economists, we'll use the one that seems most impressive: GDP per
Capita at PPP in constant 2005 dollars, which has the id "NY.GDP.PCAP.PP.KD".
But what about using high-income countries?

    >>> wbdata.get_incomelevel()
    HIC	High income
    INX	Not classified
    LIC	Low income
    LMC	Lower middle income
    LMY	Low & middle income
    MIC	Middle income
    UMC	Upper middle income

Funtastic.  Finally, let's make sure we get our data into a lovely merged
pandas DataFrame, suitable for analysis with that library, statsmodels, or
whatever else we'd like.

    >>> countries = [i['id'] for i in wbdata.get_country(incomelevel="HIC", display=False)]
    >>> indicators = {"IC.BUS.EASE.XQ": "doing_business", "NY.GDP.PCAP.PP.KD": "gdppc"}
    >>> df = wbdata.get_dataframe(indicators, country=countries, convert_date=True)
    >>> df.describe()
      doing_business          gdppc
    count  58.000000    1667.000000
    mean   50.982759   36822.636670
    std    38.092911   20908.168884
    min     1.000000    7855.027176
    25%    19.500000   22610.617293
    50%    41.000000   32990.761454
    75%    78.750000   43493.284108
    max    140.000000 135318.754421
    # doing_business is only available for 2018, and gdppc is only available for prior years,
    # so let's merge the latest of each to calculate the correlation.
    >>> df.reset_index(inplace=True)
    >>> doing_business = df[df.date == '2018-01-01'][['country', 'doing_business']]
    >>> gdppc = df[df.date == '2017-01-01'][['country', 'gdppc']]
    >>> cordf = doing_business.merge(gdppc, on='country').dropna()
    >>> cordf.gdppc.corr(cordf.doing_business)
    -0.28564209578712124

And, since lower scores on that indicator mean more business-friendly
regulations, that's exactly what we would expect. It goes without saying that
we can use our data now to do any other analysis required.


Usings Pandas DataFrames
------------------------

The ``get_dataframe()`` function returns a `DataFrame <http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe>`_ object, as implemented by the `Pandas data analysis library <https://pandas.pydata.org>`_. The dataframes returned by `wbdata` contain multiple indexes for the independent variables ``country``, ``name`` and ``date``.

The following DataFrame contains two columns, ``doing_business`` and ``gdppc``, and two indices, ``country`` and ``date``:

    >>> countries = [i['id'] for i in wbdata.get_country(incomelevel="LIC", display=False)]
    >>> indicators = {"NY.GDP.PCAP.PP.KD": "gdppc"}
    >>> df = wbdata.get_dataframe(indicators, country=countries, convert_date=True)
    >>> print df
                                gdppc
    country     date                   
    Afghanistan 2017-01-01          NaN
                2016-01-01  1739.583177
                2015-01-01  1747.978457
                2014-01-01  1780.382366
                2013-01-01  1814.155825
                2012-01-01  1839.273579
                2011-01-01  1660.739856
    ...                             ...
    Zimbabwe    1989-01-01          NaN
                1988-01-01          NaN
                1987-01-01          NaN
    ...                             ...
    [1798 rows x 1 columns]


To select data for Afganistan, we can call:

    >>> df.loc[(['Afghanistan']), :]
                              gdppc
    country     date                   
    Afghanistan 2017-01-01          NaN
                2016-01-01  1739.583177
                2015-01-01  1747.978457
                2014-01-01  1780.382366
                2013-01-01  1814.155825
                2012-01-01  1839.273579
                2011-01-01  1660.739856
                2010-01-01  1614.255001
                2009-01-01  1531.173993
                2008-01-01  1298.143159
    ...                             ...

And to select the 2016 value for all countries:

    >>> df.loc[(slice(None),['2016-01-01']), :]
                                                gdppc
    country                   date                   
    Afghanistan               2016-01-01  1739.583177
    Burundi                   2016-01-01   721.176562
    Benin                     2016-01-01  2009.961384
    Burkina Faso              2016-01-01  1642.185834
    Central African Republic  2016-01-01   647.880445
    Congo, Dem. Rep.          2016-01-01   743.894342
    Comoros                   2016-01-01  1411.152339
    Eritrea                   2016-01-01          NaN
    ...                             ...

More details about working with multiple indices can be found `here <https://pandas.pydata.org/pandas-docs/stable/advanced.html#using-slicers>`_.

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

