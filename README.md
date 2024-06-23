<h1 align=center>Kerykeion</h1>
<div align="center">
    <a href="#">
        <img src="https://img.shields.io/github/contributors/g-battaglia/kerykeion?color=blue&logo=github" alt="contributors">
    </a>
    <a href="#">
        <img src="https://img.shields.io/github/stars/g-battaglia/kerykeion.svg?logo=github" alt="stars">
    </a>
    <a href="#">
        <img src="https://img.shields.io/github/forks/g-battaglia/kerykeion.svg?logo=github" alt="forks">
    </a>
    <a href="https://pypi.org/project/kerykeion" target="_blank">
        <img src="https://visitor-badge.laobi.icu/badge?page_id=g-battaglia.kerykeion" alt="visitors"/>
    </a>
    <a href="https://pypi.org/project/kerykeion" target="_blank">
        <img src="https://img.shields.io/pypi/v/kerykeion?label=pypi%20package" alt="Package version">
    </a>
    <a href="https://pypi.org/project/kerykeion" target="_blank">
        <img src="https://img.shields.io/pypi/pyversions/kerykeion.svg" alt="Supported Python versions">
    </a>
</div>

&nbsp;

Kerykeion is a python library for Astrology.
It can calculate all the planet and house position,
also it can calculate the aspects of a single persone or between two, you can set how many planets you
need in the settings in the utility module.
It also can generate an SVG of a birthchart, a synastry chart or a transit chart.

The core goal of this project is to provide a simple and easy approach to astrology in a data driven way.

Here's an example of a birthchart:

![Kanye Birth Chart](http://centuryboy.altervista.org/KanyeNatalChart.svg)

## Web API

If you want to use Kerykeion in a web application, I've created a web API for this purpose, you can find it here:

**[AstrologerAPI](https://rapidapi.com/gbattaglia/api/astrologer/)**

It's [open source](https://github.com/g-battaglia/Astrologer-API), it's a way to support me and the project.

## Donate

Maintaining this project is a lot of work, the Astrologer API doesn't nearly cover the costs of a software engineer working on this project full time. I do this because I love it, but until I can make this my full time job, I won't be able to spend as much time on it.

If you want to support me, you can do it here:

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/kerykeion)

## Installation

Kerykeion is a _Python 3.9_ package, make sure you have _Python 3.9_ or above installed on your system.

```bash
pip3 install kerykeion
```

## Usage

Here some examples:

```python

# Import the main class for creating a kerykeion instance:
from kerykeion import AstrologicalSubject

# Create a kerykeion instance:
# Args: Name, year, month, day, hour, minuts, city, nation(optional)
kanye = AstrologicalSubject("Kanye", 1977, 6, 8, 8, 45, "Atlanta")

# Get the information about the sun in the chart:
# (The position of the planets always starts at 0)
kanye.sun

#> {'name': 'Sun', 'quality': 'Mutable', 'element': 'Air', 'sign': 'Gem', 'sign_num': 2, 'pos': 17.598992059774275, 'abs_pos': 77.59899205977428, 'emoji': '♊️', 'house': '12th House', 'retrograde': False}

# Get information about the first house:
kanye.first_house

#> {'name': 'First_House', 'quality': 'Cardinal', 'element': 'Water', 'sign': 'Can', 'sign_num': 3, 'pos': 17.995779673209114, 'abs_pos': 107.99577967320911, 'emoji': '♋️'}

# Get element of the moon sign:
kanye.moon.element

#> 'Water'

```

**To avoid connecting to GeoNames (eg. avoiding hourly limit or no internet connection) you should instance kerykeion like this:**

```python
kanye = AstrologicalSubject(
    "Kanye", 1977, 6, 8, 8, 45, lng=50, lat=50, tz_str="Europe/Rome", city="Rome"
)
```

The difference is that you have to pass the longitude, latitude and the timezone string, instead of the city and nation.
If you omit the nation, it will be set to "GB" by default, but the value is not used for calculations. It's better to set it to the correct value though.

## Generate a SVG Chart

```python
from kerykeion import AstrologicalSubject, KerykeionChartSVG

first = AstrologicalSubject("Jack", 1990, 6, 15, 15, 15, "Roma")
second = AstrologicalSubject("Jane", 1991, 10, 25, 21, 00, "Roma")

# Set the type, it can be Natal, Synastry or Transit

name = KerykeionChartSVG(first, chart_type="Synastry", second_obj=second)
name.makeSVG()
print(len(name.aspects_list))

#> Generating kerykeion object for Jack...
#> Generating kerykeion object for Jane...
#> Jack birth location: Roma, 41.89193, 12.51133
#> SVG Generated Correctly
#> 38

```

![Synastry Chart](http://centuryboy.altervista.org/JackComposite_Chart.svg)

# Report

To print a report of all the data:

```python
from kerykeion import Report, AstrologicalSubject

kanye = AstrologicalSubject("Kanye", 1977, 6, 8, 8, 45, "Atlanta")
report = Report(kanye)
report.print_report()

```

Returns:

```bash
+- Kerykeion report for Kanye -+
+----------+------+-------------+-----------+----------+
| Date     | Time | Location    | Longitude | Latitude |
+----------+------+-------------+-----------+----------+
| 8/6/1977 | 8:45 | Atlanta, US | -84.38798 | 33.749   |
+----------+------+-------------+-----------+----------+
+-----------+------+-------+------+----------------+
| Planet    | Sign | Pos.  | Ret. | House          |
+-----------+------+-------+------+----------------+
| Sun       | Gem  | 17.6  | -    | Twelfth_House  |
| Moon      | Pis  | 16.43 | -    | Ninth_House    |
| Mercury   | Tau  | 26.29 | -    | Eleventh_House |
| Venus     | Tau  | 2.03  | -    | Tenth_House    |
| Mars      | Tau  | 1.79  | -    | Tenth_House    |
| Jupiter   | Gem  | 14.61 | -    | Eleventh_House |
| Saturn    | Leo  | 12.8  | -    | Second_House   |
| Uranus    | Sco  | 8.27  | R    | Fourth_House   |
| Neptune   | Sag  | 14.69 | R    | Fifth_House    |
| Pluto     | Lib  | 11.45 | R    | Fourth_House   |
| Mean_Node | Lib  | 21.49 | R    | Fourth_House   |
| True_Node | Lib  | 22.82 | R    | Fourth_House   |
| Chiron    | Tau  | 4.17  | -    | Tenth_House    |
+-----------+------+-------+------+----------------+
+----------------+------+----------+
| House          | Sign | Position |
+----------------+------+----------+
| First_House    | Can  | 18.0     |
| Second_House   | Leo  | 9.51     |
| Third_House    | Vir  | 4.02     |
| Fourth_House   | Lib  | 3.98     |
| Fifth_House    | Sco  | 9.39     |
| Sixth_House    | Sag  | 15.68    |
| Seventh_House  | Cap  | 18.0     |
| Eighth_House   | Aqu  | 9.51     |
| Ninth_House    | Pis  | 4.02     |
| Tenth_House    | Ari  | 3.98     |
| Eleventh_House | Tau  | 9.39     |
| Twelfth_House  | Gem  | 15.68    |
+----------------+------+----------+

```

And if you want to export it to a file:

```bash
python3 your_script_name.py > file.txt
```

## Other exeples of possibles usecase

```python
# Get all aspects between two persons:

from kerykeion import SynastryAspects, AstrologicalSubject
first = AstrologicalSubject("Jack", 1990, 6, 15, 15, 15, "Roma")
second = AstrologicalSubject("Jane", 1991, 10, 25, 21, 00, "Roma")

name = SynastryAspects(first, second)
aspect_list = name.get_relevant_aspects()
print(aspect_list[0])

#> Generating kerykeion object for Jack...
#> Generating kerykeion object for Jane...
#> {'p1_name': 'Sun', 'p1_abs_pos': 84.17867971515636, 'p2_name': 'Sun', 'p2_abs_pos': 211.90472999502984, 'aspect': 'trine', 'orbit': 7.726050279873476, 'aspect_degrees': 120, 'color': '#36d100', 'aid': 6, 'diff': 127.72605027987348, 'p1': 0, 'p2': 0}

```

## Ayanamsa (Sidereal Modes)

You can set the zodiac type and the sidereal mode in the AstrologicalSubject class:

```python
johnny = AstrologicalSubject("Johnny Depp", 1963, 6, 9, 0, 0, "Owensboro", "US", zodiac_type="Sidereal", sidereal_mode="LAHIRI")
```

## Documentation

Most of the functions and the classes are self documented by the types and have docstrings.
An auto-generated documentation [is available here](https://www.kerykeion.net/pydocs/kerykeion.html).

Sooner or later I'll try to write an extensive documentation.

## Development

You can clone this repository or download a zip file using the right side buttons.

## Contributing

Feel free to contribute to the code!

## License

This project is licensed under the AGPL-3.0 License.
To understand how this impacts your use of the software, please see the [LICENSE](LICENSE) file for details.
If you have questions, you can reach out to me at my [email](mailto:battaglia.giacomo@yahoo.it?subject=Kerykeion) address.
As a rule of thumb, if you are using this library in a project, you should open source the code of the project with a compatible license.

You can implement the logic of kerykeion in your project and also keep it closed source by using a third party API, like the [AstrologerAPI](https://rapidapi.com/gbattaglia/api/astrologer/). The AstrologerAPI is AGPL-3.0 compliant. Subscribing to the API is also, currently, the best way to support the project.
