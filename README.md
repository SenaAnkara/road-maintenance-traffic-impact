# The Effect of Road Maintenance Works on Traffic

Individual computer project carried out under the supervision of a faculty advisor, Yıldız Technical University, Department of Computer Engineering (January 2024).

This project looks at how road maintenance works in Istanbul change the average vehicle speed at their location, and tries to predict the average speed during a maintenance work.

## Data

Two datasets from the Istanbul Metropolitan Municipality (İBB) were used:

- **Road maintenance announcements:** start and end times, coordinates, road segment and announcement type.
- **Traffic speed data (2019):** average speeds in five-minute intervals for each road segment and direction.

The data was obtained from İBB and is not included in this repository. To run the notebook with similar data, use the following folder structure:

```
Veriler/
  Duyuru Verileri/
    Duyurular.csv
    DuyuruTipleri.csv
  Hız Verileri/
    2019/
      {segmentId}_{direction}_2019.csv
```

## Method

1. **Matching:** Announcements were matched with speed files using the road segment ID and direction. Only road maintenance announcement types were kept.
2. **Cleaning:** Rows without direction information, an announcement that lasted about six months and an announcement with an end date before its start date were removed. 75 announcements remained.
3. **Effect ratio:** For each announcement, the average speed during the work was compared with the average speed on the same weekday and time in the surrounding weeks (3 and 5 weeks before and after, and the whole year).
4. **Analysis:** Effect ratios were compared by part of the day, day of the week, district and road. Districts and road names were found from coordinates with geopy.
5. **Five-minute segments:** Each announcement was split into five-minute parts to increase the number of samples for regression.
6. **Regression:** Random forest, KNN, decision tree and SVR models (scikit-learn) were trained to predict the average speed during maintenance. Features: average speed of the previous five weeks, part of the day, day of the week and coordinates. Parts of the same announcement were kept in the same train or test set. Random forest gave the lowest error (MAPE).
7. **Visualization:** Results were shown on interactive maps with ipyleaflet, and a simple Tkinter interface was built to display predictions.

## Notes

- After cleaning and matching, only a small number of announcements remained. For this reason classical regression models were used instead of neural networks.
- Code comments and variable names are in Turkish.
- Notebook outputs were removed because they contain parts of the original data.

## Requirements

```
pip install -r requirements.txt
```

The Tkinter interface needs to be run locally, not in Google Colab.
