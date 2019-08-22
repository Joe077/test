Usage
=====

As **trendet** is intended to be combined with **investpy**, the main functionality is to
detect trends on stock time series data so to analyse the market and which behaviour does it have
in certain date ranges.

In the example presented below, the ``identify_trends`` function will be used to detect 3 bearish trends
with a time window above 5 days, which implies that every bearish (decreasing) trend with a longer
duration than 5 days will be identified and so on added to a ``pandas.DataFrame`` which already contains
OHLC values, in a new column called `Trend` which will be labeled.

.. code-block:: python

    import trendet

    import matplotlib.pyplot as plt
    import seaborn as sns
    sns.set(style='darkgrid')


    df = trendet.identify_trends(equity='bbva',
                                 from_date='01/01/2018',
                                 to_date='01/01/2019',
                                 window_size=5,
                                 trend_limit=3,
                                 labels=['A', 'B', 'C'])

    df.reset_index(inplace=True)

    with plt.style.context('paper'):
        plt.figure(figsize=(20, 10))

        ax = sns.lineplot(x=df['Date'], y=df['Close'])

        for label in ['A', 'B', 'C']:
            sns.lineplot(x=df[df['Trend'] == label]['Date'], y=df[df['Trend'] == label]['Close'], color='red')
            ax.axvspan(df[df['Trend'] == label]['Date'].iloc[0], df[df['Trend'] == label]['Date'].iloc[-1], alpha=0.1, color='red')

        plt.show()

So on, the resulting plot which will be outputted from the previous block of code will look like:

.. image:: https://raw.githubusercontent.com/alvarob96/trendet/master/docs/trendet_example.png
    :align: center