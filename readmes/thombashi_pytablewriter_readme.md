.. contents:: **pytablewriter**
   :backlinks: top
   :depth: 2

Summary
=========
`pytablewriter <https://github.com/thombashi/pytablewriter>`__ is a Python library to write a table in various formats: AsciiDoc / CSV / Elasticsearch / HTML / JavaScript / JSON / LaTeX / LDJSON / LTSV / Markdown / MediaWiki / NumPy / Excel / Pandas / Python / reStructuredText / SQLite / TOML / TSV / YAML.

.. image:: https://badge.fury.io/py/pytablewriter.svg
    :target: https://badge.fury.io/py/pytablewriter
    :alt: PyPI package version

.. image:: https://anaconda.org/conda-forge/pytablewriter/badges/version.svg
    :target: https://anaconda.org/conda-forge/pytablewriter
    :alt: conda-forge package version

.. image:: https://img.shields.io/pypi/pyversions/pytablewriter.svg
    :target: https://pypi.org/project/pytablewriter/
    :alt: Supported Python versions

.. image:: https://img.shields.io/pypi/implementation/pytablewriter.svg
    :target: https://pypi.org/project/pytablewriter
    :alt: Supported Python implementations

.. image:: https://github.com/thombashi/pytablewriter/actions/workflows/lint_and_test.yml/badge.svg
    :target: https://github.com/thombashi/pytablewriter/actions/workflows/lint_and_test.yml
    :alt: CI status of Linux/macOS/Windows

.. image:: https://coveralls.io/repos/github/thombashi/pytablewriter/badge.svg?branch=master
    :target: https://coveralls.io/github/thombashi/pytablewriter?branch=master
    :alt: Test coverage

.. image:: https://github.com/thombashi/pytablewriter/actions/workflows/codeql-analysis.yml/badge.svg
    :target: https://github.com/thombashi/pytablewriter/actions/workflows/codeql-analysis.yml
    :alt: CodeQL

Features
--------
- Write a table in various formats:
    - Text formats:
        - `AsciiDoc <https://asciidoc.org/>`__
        - CSV / Tab-separated values (TSV) / Space-separated values (SSV)
        - HTML / CSS
        - JSON / `Line-delimited JSON(LDJSON) <https://en.wikipedia.org/wiki/JSON_streaming#Line-delimited_JSON>`__
        - `Labeled Tab-separated Values (LTSV) <http://ltsv.org/>`__
        - LaTeX: ``tabular``/``array`` environment
        - Markdown: CommonMark / `GitHub Flavored Markdown (GFM) <https://github.github.com/gfm/>`__ / `kramdown <https://kramdown.gettalong.org/>`__
        - `MediaWiki <https://www.mediawiki.org/wiki/MediaWiki>`__
        - reStructuredText: `Grid Tables <http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#grid-tables>`__/`Simple Tables <http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#simple-tables>`__/`CSV Table <http://docutils.sourceforge.net/docs/ref/rst/directives.html#id4>`__
        - Source code (definition of a variable that represents tabular data)
            - JavaScript / `NumPy <https://www.numpy.org/>`__ (`numpy.array <https://docs.scipy.org/doc/numpy/reference/generated/numpy.array.html>`__) / `Pandas <https://pandas.pydata.org/>`__ (`pandas.DataFrame <https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html>`__) / Python
        - `TOML <https://github.com/toml-lang/toml>`__
        - `YAML <https://yaml.org/>`__
        - Unicode
    - Binary file formats:
        - Microsoft Excel :superscript:`TM` (``.xlsx``/``.xls`` file format)
        - `pandas.DataFrame <https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html>`__ pickle file
        - `SQLite <https://www.sqlite.org/index.html>`__ database
    - Application specific formats:
        - `Elasticsearch <https://www.elastic.co/products/elasticsearch>`__
- Automatic table cell formatting:
    - Alignment
    - Padding
    - Decimal places of numbers
- Customize table cell styles:
    - Text/Background color
    - Text alignment
    - Font size/weight
    - Thousand separator for numbers: e.g. ``1,000``/``1 000``
- Configure output:
    - Write table to a stream such as a file/standard-output/string-buffer/Jupyter-Notebook
    - Get rendered tabular text
- Data sources:
    - nested list
    - CSV
    - `pandas.DataFrame <https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html>`__ / `pandas.Series <https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html>`__
    - etc.
- Multibyte character support
- ANSI color support

Installation
============

Installation: pip
------------------------------
::

    pip install pytablewriter

Some of the formats require additional dependency packages, you can install these packages as follows:

.. csv-table:: Installation of optional dependencies
    :header: Installation example, Remark

    ``pip install pytablewriter[es]``, Elasticsearch
    ``pip install pytablewriter[excel]``, Excel
    ``pip install pytablewriter[html]``, HTML
    ``pip install pytablewriter[sqlite]``, SQLite database
    ``pip install pytablewriter[toml]``, TOML
    ``pip install pytablewriter[theme]``, pytablewriter theme plugins
    ``pip install pytablewriter[all]``, Install all of the optional dependencies

Installation: conda
------------------------------
::

    conda install -c conda-forge pytablewriter

Installation: apt
------------------------------
::

    sudo add-apt-repository ppa:thombashi/ppa
    sudo apt update
    sudo apt install python3-pytablewriter

Examples
==========
Write tables
--------------
Write a Markdown table
~~~~~~~~~~~~~~~~~~~~~~~~
:Sample Code:
    .. code-block:: python

        from pytablewriter import MarkdownTableWriter

        def main():
            writer = MarkdownTableWriter(
                table_name="example_table",
                headers=["int", "float", "str", "bool", "mix", "time"],
                value_matrix=[
                    [0,   0.1,      "hoge", True,   0,      "2017-01-01 03:04:05+0900"],
                    [2,   "-2.23",  "foo",  False,  None,   "2017-12-23 45:01:23+0900"],
                    [3,   0,        "bar",  "true",  "inf", "2017-03-03 33:44:55+0900"],
                    [-10, -9.9,     "",     "FALSE", "nan", "2017-01-01 00:00:00+0900"],
                ],
            )
            writer.write_table()

        if __name__ == "__main__":
            main()

:Output:
    .. code-block::

        # example_table
        |int|float|str |bool |  mix   |          time          |
        |--:|----:|----|-----|-------:|------------------------|
        |  0| 0.10|hoge|True |       0|2017-01-01 03:04:05+0900|
        |  2|-2.23|foo |False|        |2017-12-23 12:34:51+0900|
        |  3| 0.00|bar |True |Infinity|2017-03-03 22:44:55+0900|
        |-10|-9.90|    |False|     NaN|2017-01-01 00:00:00+0900|

:Rendering Result:
    .. figure:: https://cdn.jsdelivr.net/gh/thombashi/pytablewriter@master/docs/pages/examples/table_format/text/ss/markdown.png
       :scale: 80%
       :alt: https://github.com/thombashi/pytablewriter/blob/master/docs/pages/examples/table_format/text/ss/markdown.png

       Rendered markdown at GitHub

Write a Markdown table with a margin
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
:Sample Code:
    .. code-block:: python

        from pytablewriter import MarkdownTableWriter

        def main():
            writer = MarkdownTableWriter(
                table_name="write example with a margin",
                headers=["int", "float", "str", "bool", "mix", "time"],
                value_matrix=[
                    [0,   0.1,      "hoge", True,   0,      "2017-01-01 03:04:05+0900"],
                    [2,   "-2.23",  "foo",  False,  None,   "2017-12-23 45:01:23+0900"],
                    [3,   0,        "bar",  "true",  "inf", "2017-03-03 33:44:55+0900"],
                    [-10, -9.9,     "",     "FALSE", "nan", "2017-01-01 00:00:00+0900"],
                ],
                margin=1  # add a whitespace for both sides of each cell
            )
            writer.write_table()

        if __name__ == "__main__":
            main()

:Output:
    .. code-block::

        # write example with a margin
        | int | float | str  | bool  |   mix    |           time           |
        | --: | ----: | ---- | ----- | -------: | ------------------------ |
        |   0 |  0.10 | hoge | True  |        0 | 2017-01-01 03:04:05+0900 |
        |   2 | -2.23 | foo  | False |          | 2017-12-23 12:34:51+0900 |
        |   3 |  0.00 | bar  | True  | Infinity | 2017-03-03 22:44:55+0900 |
        | -10 | -9.90 |      | False |      NaN | 2017-01-01 00:00:00+0900 |

``margin`` attribute can be available for all of the text format writer classes.

Write a Markdown table to a stream or a file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`Refer an example <https://github.com/thombashi/pytablewriter/blob/master/examples/py/stream/configure_stream.py>`__

Write a table to an Excel sheet
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
:Sample Code:
    .. code-block:: python

        from pytablewriter import ExcelXlsxTableWriter

        def main():
            writer = ExcelXlsxTableWriter()
            writer.table_name = "example"
            writer.headers = ["int", "float", "str", "bool", "mix", "time"]
            writer.value_matrix = [
                [0,   0.1,      "hoge", True,   0,      "2017-01-01 03:04:05+0900"],
                [2,   "-2.23",  "foo",  False,  None,   "2017-12-23 12:34:51+0900"],
                [3,   0,        "bar",  "true",  "inf", "2017-03-03 22:44:55+0900"],
                [-10, -9.9,     "",     "FALSE", "nan", "2017-01-01 00:00:00+0900"],
            ]
            writer.dump("sample.xlsx")

        if __name__ == "__main__":
            main()

:Output:
    .. figure:: https://cdn.jsdelivr.net/gh/thombashi/pytablewriter@master/docs/pages/examples/table_format/binary/spreadsheet/ss/excel_single.png
       :scale: 100%
       :alt: https://github.com/thombashi/pytablewriter/blob/master/docs/pages/examples/table_format/binary/spreadsheet/ss/excel_single.png

       Output excel file (``sample_single.xlsx``)

Write a Unicode table
~~~~~~~~~~~~~~~~~~~~~~~
:Sample Code:
    .. code-block:: python

        from pytablewriter import UnicodeTableWriter

        def main():
            writer = UnicodeTableWriter(
                table_name="example_table",
                headers=["int", "float", "str", "bool", "mix", "time"],
                value_matrix=[
                    [0,   0.1,      "hoge", True,   0,      "2017-01-01 03:04:05+0900"],
                    [2,   "-2.23",  "foo",  False,  None,   "2017-12-23 45:01:23+0900"],
                    [3,   0,        "bar",  "true",  "inf", "2017-03-03 33:44:55+0900"],
                    [-10, -9.9,     "",     "FALSE", "nan", "2017-01-01 00:00:00+0900"],
                ]
            )
            writer.write_table()

        if __name__ == "__main__":
            main()

:Output:
    .. code-block::

        ┌───┬─────┬────┬─────┬────────┬────────────────────────┐
        │int│float│str │bool │  mix   │          time          │
        ├───┼─────┼────┼─────┼────────┼────────────────────────┤
        │  0│ 0.10│hoge│True │       0│2017-01-01 03:04:05+0900│
        ├───┼─────┼────┼─────┼────────┼────────────────────────┤
        │  2│-2.23│foo │False│        │2017-12-23 12:34:51+0900│
        ├───┼─────┼────┼─────┼────────┼────────────────────────┤
        │  3│ 0.00│bar │True │Infinity│2017-03-03 22:44:55+0900│
        ├───┼─────┼────┼─────┼────────┼────────────────────────┤
        │-10│-9.90│    │False│     NaN│2017-01-01 00:00:00+0900│
        └───┴─────┴────┴─────┴────────┴────────────────────────┘

Write a table with JavaScript format (as a nested list variable definition)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
:Sample Code:
    .. code-block:: python

        import pytablewriter as ptw


        def main():
            writer = ptw.JavaScriptTableWriter(
                table_name="js_variable",
                headers=["int", "float", "str", "bool", "mix", "time"],
                value_matrix=[
                    [0, 0.1, "hoge", True, 0, "2017-01-01 03:04:05+0900"],
                    [2, "-2.23", "foo", False, None, "2017-12-23 45:01:23+0900"],
                    [3, 0, "bar", "true", "inf", "2017-03-03 33:44:55+0900"],
                    [-10, -9.9, "", "FALSE", "nan", "2017-01-01 00:00:00+0900"],
                ],
            )

            writer.write_table()


        if __name__ == "__main__":
            main()

:Output:
    .. code-block:: js

        const js_variable = [
            ["int", "float", "str", "bool", "mix", "time"],
            [0, 0.1, "hoge", true, 0, "2017-01-01 03:04:05+0900"],
            [2, -2.23, "foo", false, null, "2017-12-23 45:01:23+0900"],
            [3, 0, "bar", true, Infinity, "2017-03-03 33:44:55+0900"],
            [-10, -9.9, "", "FALSE", NaN, "2017-01-01 00:00:00+0900"]
        ];

Write a Markdown table from ``pandas.DataFrame`` instance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
``from_dataframe`` method of writer classes will set up tabular data from ``pandas.DataFrame``:

:Sample Code:
    .. code-block:: python

        from textwrap import dedent
        import pandas as pd
        import io
        from pytablewriter import MarkdownTableWriter

        def main():
            csv_data = io.StringIO(dedent("""\
                "i","f","c","if","ifc","bool","inf","nan","mix_num","time"
                1,1.10,"aa",1.0,"1",True,Infinity,NaN,1,"2017-01-01 00:00:00+09:00"
                2,2.20,"bbb",2.2,"2.2",False,Infinity,NaN,Infinity,"2017-01-02 03:04:05+09:00"
                3,3.33,"cccc",-3.0,"ccc",True,Infinity,NaN,NaN,"2017-01-01 00:00:00+09:00"
                """))
            df = pd.read_csv(csv_data, sep=',')

            writer = MarkdownTableWriter(dataframe=df)
            writer.write_table()

        if __name__ == "__main__":
            main()

:Output:
    .. code-block::

        | i | f  | c  | if |ifc|bool |  inf   |nan|mix_num |          time           |
        |--:|---:|----|---:|---|-----|--------|---|-------:|-------------------------|
        |  1|1.10|aa  | 1.0|  1|True |Infinity|NaN|       1|2017-01-01 00:00:00+09:00|
        |  2|2.20|bbb | 2.2|2.2|False|Infinity|NaN|Infinity|2017-01-02 03:04:05+09:00|
        |  3|3.33|cccc|-3.0|ccc|True |Infinity|NaN|     NaN|2017-01-01 00:00:00+09:00|


Adding a column of the DataFrame index if you specify ``add_index_column=True``:

:Sample Code:
    .. code-block:: python

        import pandas as pd
        import pytablewriter as ptw

        def main():
            writer = ptw.MarkdownTableWriter(table_name="add_index_column")
            writer.from_dataframe(
                pd.DataFrame({"A": [1, 2], "B": [10, 11]}, index=["a", "b"]),
                add_index_column=True,
            )
            writer.write_table()

        if __name__ == "__main__":
            main()

:Output:
    .. code-block::

        # add_index_column
        |   | A | B |
        |---|--:|--:|
        |a  |  1| 10|
        |b  |  2| 11|

Write a markdown table from a space-separated values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
:Sample Code:
    .. code-block:: python

        import pytablewriter as ptw


        def main():
            writer = ptw.MarkdownTableWriter(table_name="ps")
            writer.from_csv(
                """
                USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
                root         1  0.0  0.4  77664  8784 ?        Ss   May11   0:02 /sbin/init
                root         2  0.0  0.0      0     0 ?        S    May11   0:00 [kthreadd]
                root         4  0.0  0.0      0     0 ?        I<   May11   0:00 [kworker/0:0H]
                root         6  0.0  0.0      0     0 ?        I<   May11   0:00 [mm_percpu_wq]
                root         7  0.0  0.0      0     0 ?        S    May11   0:01 [ksoftirqd/0]
                """,
                delimiter=" ",
            )
            writer.write_table()


        if __name__ == "__main__":
            main()

:Output:
    .. code-block::

        # ps
        |USER|PID|%CPU|%MEM| VSZ |RSS |TTY|STAT|START|TIME|   COMMAND    |
        |----|--:|---:|---:|----:|---:|---|----|-----|----|--------------|
        |root|  1|   0| 0.4|77664|8784|?  |Ss  |May11|0:02|/sbin/init    |
        |root|  2|   0| 0.0|    0|   0|?  |S   |May11|0:00|[kthreadd]    |
        |root|  4|   0| 0.0|    0|   0|?  |I<  |May11|0:00|[kworker/0:0H]|
        |root|  6|   0| 0.0|    0|   0|?  |I<  |May11|0:00|[mm_percpu_wq]|
        |root|  7|   0| 0.0|    0|   0|?  |S   |May11|0:01|[ksoftirqd/0] |

Get rendered tabular text as str
----------------------------------
``dumps`` method returns rendered tabular text.
``dumps`` only available for text format writers.

:Sample Code:
    .. code-block:: python

        import pytablewriter as ptw


        def main():
            writer = ptw.MarkdownTableWriter(
                headers=["int", "float", "str", "bool", "mix", "time"],
                value_matrix=[
                    [0, 0.1, "hoge", True, 0, "2017-01-01 03:04:05+0900"],
                    [2, "-2.23", "foo", False, None, "2017-12-23 45:01:23+0900"],
                    [3, 0, "bar", "true", "inf", "2017-03-03 33:44:55+0900"],
                    [-10, -9.9, "", "FALSE", "nan", "2017-01-01 00:00:00+0900"],
                ],
            )

            print(writer.dumps())


        if __name__ == "__main__":
            main()

:Output:
    .. code-block::

        |int|float|str |bool |  mix   |          time          |
        |--:|----:|----|-----|-------:|------------------------|
        |  0| 0.10|hoge|True |       0|2017-01-01 03:04:05+0900|
        |  2|-2.23|foo |False|        |2017-12-23 45:01:23+0900|
        |  3| 0.00|bar |True |Infinity|2017-03-03 33:44:55+0900|
        |-10|-9.90|    |False|     NaN|2017-01-01 00:00:00+0900|

Configure table styles
------------------------
Column styles
~~~~~~~~~~~~~~~
Writers can specify
`Style <https://pytablewriter.rtfd.io/en/latest/pages/reference/style.html>`__
for each column by ``column_styles`` attribute of writer classes.

:Sample Code:
    .. code-block:: python

        import pytablewriter as ptw
        from pytablewriter.style import Style


        def main():
            writer = ptw.MarkdownTableWriter(
                table_name="set style by column_styles",
                headers=[
                    "auto align",
                    "left align",
                    "center align",
                    "bold",
                    "italic",
                    "bold italic ts",
                ],
                value_matrix=[
                    [11, 11, 11, 11, 11, 11],
                    [1234, 1234, 1234, 1234, 1234, 1234],
                ],
                column_styles=[
                    Style(),
                    Style(align="left"),
                    Style(align="center"),
                    Style(font_weight="bold"),
                    Style(font_style="italic"),
                    Style(font_weight="bold", font_style="italic", thousand_separator=","),
                ],  # specify styles for each column
            )
            writer.write_table()


        if __name__ == "__main__":
            main()

:Output:
    .. code-block::

        # set style by styles
        |auto align|left align|center align|  bold  |italic|bold italic ts|
        |---------:|----------|:----------:|-------:|-----:|-------------:|
        |        11|11        |     11     |  **11**|  _11_|      _**11**_|
        |      1234|1234      |    1234    |**1234**|_1234_|   _**1,234**_|

    `Rendering result <https://github.com/thombashi/pytablewriter/tree/master/docs/pages/examples/style/output.md>`__


You can also set ``Style`` to a specific column with index or header by using ``set_style`` method:

:Sample Code:
    .. code-block:: python

        from pytablewriter import MarkdownTableWriter
        from pytablewriter.style import Style

        def main():
            writer = MarkdownTableWriter()
            writer.headers = ["A", "B", "C",]
            writer.value_matrix = [[11, 11, 11], [1234, 1234, 1234]]

            writer.table_name = "set style by column index"
            writer.set_style(1, Style(align="center", font_weight="bold"))
            writer.set_style(2, Style(thousand_separator=" "))
            writer.write_table()
            writer.write_null_line()

            writer.table_name = "set style by header"
            writer.set_style("B", Style(font_style="italic"))
            writer.write_table()

        if __name__ == "__main__":
            main()

:Output:
    .. code-block::

        # set style by column index
        | A  |   B    |  C  |
        |---:|:------:|----:|
        |  11| **11** |   11|
        |1234|**1234**|1 234|

        # set style by header
        | A  |  B   |  C  |
        |---:|-----:|----:|
        |  11|  _11_|   11|
        |1234|_1234_|1 234|

Style filter
~~~~~~~~~~~~~~
The following command will install external predefined themes:
::

    pip install pytablewriter[theme]

``theme`` argument of writer constructor or ``set_theme`` method can set"" predefined style filters.
``altrow`` theme will colored rows alternatively:

:Sample Code:
    .. code-block:: python

        import pytablewriter as ptw

        writer = ptw.TableWriterFactory.create_from_format_name(
            "markdown",
            headers=["INT", "STR"],
            value_matrix=[[1, "hoge"], [2, "foo"], [3, "bar"]],
            margin=1,
            theme="altrow",
        )
        writer.write_table()

:Output:
    .. figure:: https://cdn.jsdelivr.net/gh/thombashi/pytablewriter-altrow-theme@master/ss/ptw-altrow-theme_example_default.png
       :scale: 100%
       :alt: https://github.com/thombashi/pytablewriter-altrow-theme/blob/master/ss/ptw-altrow-theme_example_default.png

Themes can be created as plugins like as follows:
https://github.com/thombashi/pytablewriter-altrow-theme

Make tables for specific applications
---------------------------------------
Render a table on Jupyter Notebook
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
https://nbviewer.jupyter.org/github/thombashi/pytablewriter/blob/master/examples/ipynb/jupyter_notebook_example.ipynb

.. figure:: https://cdn.jsdelivr.net/gh/thombashi/pytablewriter@master/docs/pages/examples/jupyter_notebook/ss/jupyter_notebook.png
   :scale: 100%
   :alt: https://github.com/thombashi/pytablewriter/blob/master/docs/pages/examples/jupyter_notebook/ss/jupyter_notebook.png

   Table formatting for Jupyter Notebook

Multibyte character support
-----------------------------
Write a table using multibyte character
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can use multibyte characters as table data.
Multibyte characters also properly padded and aligned.

:Sample Code:
    .. code-block:: python

        import pytablewriter as ptw


        def main():
            writer = ptw.RstSimpleTableWriter(
                table_name="生成に関するパターン",
                headers=["パターン名", "概要", "GoF", "Code Complete[1]"],
                value_matrix=[
                    ["Abstract Factory", "関連する一連のインスタンスを状況に応じて、適切に生成する方法を提供する。", "Yes", "Yes"],
                    ["Builder", "複合化されたインスタンスの生成過程を隠蔽する。", "Yes", "No"],
                    ["Factory Method", "実際に生成されるインスタンスに依存しない、インスタンスの生成方法を提供する。", "Yes", "Yes"],
                    ["Prototype", "同様のインスタンスを生成するために、原型のインスタンスを複製する。", "Yes", "No"],
                    ["Singleton", "あるクラスについて、インスタンスが単一であることを保証する。", "Yes", "Yes"],
                ],
            )
            writer.write_table()


        if __name__ == "__main__":
            main()

:Output:
    .. figure:: https://cdn.jsdelivr.net/gh/thombashi/pytablewriter@master/docs/pages/examples/multibyte/ss/multi_byte_char.png
       :scale: 100%
       :alt: https://github.com/thombashi/pytablewriter/blob/master/docs/pages/examples/multibyte/ss/multi_byte_char.png

       Output of multi-byte character table

Multi processing
------------------
You can increase the number of workers to process table data via ``max_workers`` attribute of a writer.
The more ``max_workers`` the less processing time when tabular data is large and the execution environment has available cores.

if you increase ``max_workers`` larger than one, recommend to use main guarded as follows to avoid problems caused by multi processing:

.. code-block:: python

    from multiprocessing import cpu_count
    import pytablewriter as ptw

    def main():
        writer = ptw.MarkdownTableWriter()
        writer.max_workers = cpu_count()
        ...

    if __name__ == "__main__":
        main()

For more information
----------------------
More examples are available at 
https://pytablewriter.rtfd.io/en/latest/pages/examples/index.html

Dependencies
============
- Python 3.6+
- `Python package dependencies (automatically installed) <https://github.com/thombashi/pytablewriter/network/dependencies>`__


Optional dependencies
---------------------
- ``logging`` extras
    - `loguru <https://github.com/Delgan/loguru>`__: Used for logging if the package installed
- ``from`` extras
    - `pytablereader <https://github.com/thombashi/pytablereader>`__
- ``es`` extra
    - `elasticsearch <https://github.com/elastic/elasticsearch-py>`__
- ``excel`` extras
    - `xlwt <http://www.python-excel.org/>`__
    - `XlsxWriter <https://github.com/jmcnamara/XlsxWriter>`__
- ``html`` extras
    - `dominate <https://github.com/Knio/dominate/>`__
- ``sqlite`` extras
    - `SimpleSQLite <https://github.com/thombashi/SimpleSQLite>`__
- ``theme`` extras
    - `pytablewriter-altrow-theme <https://github.com/thombashi/pytablewriter-altrow-theme>`__
- ``toml`` extras
    - `toml <https://github.com/uiri/toml>`__

Documentation
===============
https://pytablewriter.rtfd.io/

Projects using pytablewriter
==================================
- `pytest-md-report <https://github.com/thombashi/pytest-md-report>`__


Related Projects
==================================
- `pytablereader <https://github.com/thombashi/pytablereader>`__
    - Tabular data loaded by ``pytablereader`` can be written another tabular data format with ``pytablewriter``.

Sponsors
====================================
.. image:: https://avatars.githubusercontent.com/u/44389260?s=48&u=6da7176e51ae2654bcfd22564772ef8a3bb22318&v=4
   :target: https://github.com/chasbecker
   :alt: Charles Becker (chasbecker)
.. image:: https://avatars.githubusercontent.com/u/46711571?s=48&u=57687c0e02d5d6e8eeaf9177f7b7af4c9f275eb5&v=4
   :target: https://github.com/Arturi0
   :alt: onetime: Arturi0
.. image:: https://avatars.githubusercontent.com/u/3658062?s=48&v=4
   :target: https://github.com/b4tman
   :alt: onetime: Dmitry Belyaev (b4tman)

`Become a sponsor <https://github.com/sponsors/thombashi>`__

