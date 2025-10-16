==============
pdfCropMargins
==============

pdfCropMargins 程序是一个命令行应用程序，用于自动裁剪 PDF 文件的边距。裁剪边距可以使 PDF 文档的页面更容易阅读——无论是打印还是在屏幕上显示时——因为显示字体会更大。该程序类似于 Perl 脚本 pdfcrop，但具有更多选项。

特性
========

- 自动检测边距，并可裁剪给定百分比的边距。
- 可以将所有页面裁剪为相同大小，以获得统一的外观（例如双页排版）。
- 支持 Linux、Windows、Cygwin 和 OS X/Darwin。
- 具有可选的图形用户界面（GUI），用于交互式裁剪。
- 通过渲染和分析页面图像来查找边界框，这使其能够处理嘈杂的扫描 PDF。
- 默认实现了一个基本的“撤销”功能。
- 可以根据第 n 小的裁剪值对页面进行均匀裁剪，这有助于处理噪声图像或少数页面边缘有不需要标记的文档。
- 可以自动运行文档预览器查看输出文件。
- 自动生成的输出文件名格式易于修改。
- 如果可能，保留文档目录信息，如书签。
- 根据文档查看器中的显示方式裁剪旋转的页面。
- 可以处理至少简单的密码加密文件。
- 可以使用 MuPDF、pdftoppm 程序或 Ghostscript 程序来查找边界框。
- 可以自动应用 Ghostscript 修复操作，尝试修复损坏的 PDF 文件。

此 GIF 显示了可选的 GUI 在裁剪文档前后的效果：

.. image:: https://user-images.githubusercontent.com/1791335/63413846-9c9e3400-c3c8-11e9-90f5-6e429ae2d74b.gif
    :width: 450px
    :align: center
    :alt: [GIF of pdfCropMargins]

最新动态
==========

请参阅 `CHANGELOG <https://github.com/abarker/pdfCropMargins/blob/master/CHANGELOG.rst>`_ 以了解最近的更改和新功能。

* 版本 2.1.4 扩展了统一和相同页面大小选项，允许选择单个边距。选项是 ``--uniform4`` 和 ``--samePageSize4``，两者都接受四个字符，每个字符为 't' 或 'f'。它还添加了 ``--replaceOriginal`` 选项，与 ``--modifyOriginal`` 相同，但不会创建备份副本。

* 版本 2.1.2 新增了在裁剪后居中文本的选项。它们是 ``--centerText``、``--centerTextHoriz``、``--centerTextVert`` 和 ``--centeringStrict``。

* 在版本 2.0.1 中，选项 ``--setSamePageSize``（``-ssp``）允许传入自定义页面框大小，而不是让程序计算最大的包含框大小。

安装 
==========

安装 pdfCropMargins 程序最简单的方法是使用 pip。

基本功能开箱即用，部分选项需要外部程序 pdftoppm 或 Ghostscript。有关在 Linux 和 Windows 上安装这些程序的信息，请参见：
`Installing pdftoppm and/or Ghostscript <https://github.com/abarker/pdfCropMargins/tree/master/doc/installing_pdftoppm_and_ghostscript.rst>`_

Linux/Ubuntu
------------

如果通过 pip 使用 ``--user`` 选项进行安装，请确保 ``$HOME/.local/bin`` 在系统的 ``PATH`` 中。（要无 ``--user`` 选项进行系统范围安装，下面的 ``pip3`` 命令需要使用 ``sudo`` 运行。）

.. code-block:: sh

   sudo apt install python3-tk
   sudo apt install ghostscript poppler-utils # 可选，用于 ghostscript 和 pdftoppm。
   pip3 install pdfCropMargins --user --upgrade

**故障排除：** 如果在 PyMuPDF 安装中遇到问题，首先要尝试升级您的 pip 程序，然后重试：

.. code-block:: sh

   pip3 install pip --upgrade --user

如果仍然收到错误 "Failed building wheel for pymupdf"，并且带有 GUI 安装，则可以尝试强制安装 PyMuPDF 的二进制版本：

.. code-block:: sh

   pip3 install pdfCropMargins --user --upgrade --only-binary pymupdf

Windows
-------

安装命令是：

.. code-block:: sh

   pip install pdfCropMargins --upgrade

``pip`` 程序应随 Python 一起自动安装。如果您找不到 pip 可执行文件，通常可以这样运行：

.. code-block:: sh

   python -m pip <arguments-to-pip>

请注意，在某些 Windows 安装中，Python 的可执行文件是 ``py`` 而不是 ``python``。

为了使命令 ``pdfcropmargins`` 能够从命令行工作，Python 的 ``bin`` 目录必须在 Windows ``Path`` 中。如果在安装 Python 时勾选了修改 ``Path`` 的选项，则系统范围的 Python ``bin`` 目录应已位于路径中；否则，应将其添加进去。（请注意，如果使用 pip 的 ``--user`` 选项进行安装，则需要确保本地 Python ``bin`` 目录在 Windows ``Path`` 中。）

运行
=======

该程序可以从以下方式运行：1) 命令行，2) 带 GUI 的命令行，3) Python 程序，或 3) 源代码仓库。

从命令行运行
-----------------------------

通过 pip 安装后，可以通过命令 ``pdfcropmargins`` 或命令 ``pdf-crop-margins`` 运行该程序。例如：

.. code-block:: sh

   pdfcropmargins -v -s -u your-file.pdf

该命令打印详细输出，强制所有页面大小相同（``-s``），然后按相同量裁剪每个页面（``-u``），以获得统一的外观，保留默认的 10% 边距。要使用 GUI 进行微调，运行相同的命令：

.. code-block:: sh

   pdfcropmargins -v -s -u -gui your-file.pdf

要获取帮助并查看可用的许多命令行选项，请运行：

.. code-block:: sh

   pdfcropmargins -h | more

该命令的完整输出也列在本文档底部。在 Windows 上，您可能需要显式地将 Python 发行版的 ``Scripts`` 目录添加到环境 ``PATH`` 中，以避免使用完整路径名。

使用 GUI 运行
--------------------

要运行 GUI，假设已安装了 pdfCropMargins 版本，只需额外传递 ``-gui`` 标志以及任何其他标志即可。该程序仍然是一个命令行应用程序，并且仍然尊重所有标志，但 GUI 允许您微调一些命令行参数的值，例如裁剪百分比等。输出文件名等均与命令行版本相同。可以从 GUI 修改的选项最初设置为命令行上传递的任何值。

图形显示在您点击“裁剪”按钮时显示每次裁剪的效果。对于同一文档的多次裁剪调用通常更快，因为 PDF 页面通常只需要渲染成图像一次。

Python 接口
----------------

该程序也可以从用户的 Python 程序中调用（当 ``pdfCropMargins`` 包可在 Python 路径中发现时）。只需导入 ``crop`` 函数，然后用包含通常命令行参数的列表分别调用它。例如：
  
.. code-block:: python

   from pdfCropMargins import crop
   crop(["-p", "20", "-u", "-s", "paper1.pdf"])
   crop(["-p", "0", "-gui", "paper2.pdf"])

任何必要的异常处理应由调用代码执行。代码可能会调用 ``sys.exit``，因此可能需要检查 ``SystemExit`` 或 ``BaseException``。

``crop`` 函数总是返回四个值，其中一些可能设置为 ``None``：

* 输出文件路径，

* 退出代码，

* 写入标准输入的文本，

* 写入标准输出的文本。

如果关键字参数 ``string_io`` 设置为 true（默认为 false），则标准输出和标准错误流会被临时重定向以捕获任何输出文本作为字符串，这些字符串作为最后两个参数返回。否则，这些值设置为 ``None``。关键字参数 ``quiet`` 意味着 ``string_io`` 参数，但在 ``crop`` 函数运行时不向终端回显任何内容。

示例用法：

.. code-block:: python

   output_doc_pathname, exit_code, stdout_str, stderr_str = crop(
                            ["-p4", "0", "10", "0", "10", "paper2.pdf"],
                            string_io=True, quiet=False)

从源分发运行
------------------------------------

pdfCropMargins 程序可以直接从源代码目录树运行，前提是已安装依赖项。只需克隆存储库并在 ``bin`` 目录中运行程序 ``pdfCropMargins.py``。

要从克隆的存储库而非 PyPI 安装程序及其依赖项，只需进入源代码根目录并运行 ``pip install .``（通常，对于代码开发，请使用 ``-e`` 选项使代码可编辑。）

获得良好的裁剪效果
------------------

* 要诊断意外裁剪，请查看最小 delta 值的页面，如详细输出（``-v``）或 GUI 底部所示。这会告诉您哪个页面的某个边距的裁剪量最小。

* 不使用 ``-u`` 或 ``-s`` 选项运行将分别裁剪每个页面，因此您可以查看哪些页面可能导致问题（例如，边缘附近有噪声或页边距文本的页面）。

* 要使裁剪后的页面大小相同，请确保同时使用 ``-s`` 首先使页面大小相同，然后使用 ``-u`` 以相同量裁剪每个页面。

* 有时需要一个小的预裁剪（``-ap`` 或 ``-ap4``）以绕过页面边缘附近的微小、不需要的标记。

文档
=============

.. 在 vim 中使用以下命令获取输出：
       :read !pdf-crop-margins -h

要查看文档，请运行::

   pdf-crop-margins -h | more

该命令的输出如下：

   Usage: pdfcropmargins [-h] [-o OUTFILE_PATH_OR_DIR] [-v] [-gui] [-p PCT]
                         [-p4 PCT PCT PCT PCT] [-pt] [-a BP] [-a4 BP BP BP BP]
                         [-cs] [-csm4 BP BP BP BP] [-ap BP] [-ap4 BP BP BP BP]
                         [-u] [-u4 {t,f} {t,f} {t,f} {t,f}] [-m INT]
                         [-m4 INT INT INT INT] [-mp INT] [-s]
                         [-s4 {t,f} {t,f} {t,f} {t,f}] [-ms INT]
                         [-ssp FLOAT FLOAT FLOAT FLOAT] [-e] [-g PAGESTR]
                         [-c [d|m|p|gr|gb|o]] [-gs] [-gsr] [-t BYTEVAL] [-nb INT]
                         [-ns INT] [-x DPI] [-y DPI] [-sr STR] [-gf INT]
                         [-b [m|c|t|a|b]] [-f [m|c|t|a|b]] [-r] [-A] [-gsf] [-nc]
                         [-pv PROG] [-mo] [-q] [-ro] [-nco] [-pf] [-sc STR]
                         [-su STR] [-ss STR] [-pw PASSWD] [-pc] [-khc] [-kvc]
                         [-spr FLOAT:FLOAT] [-prw FLOAT FLOAT FLOAT FLOAT] [-ct]
                         [-ch] [-cv] [-cst] [-i] [-pdl] [-gsp PATH] [-ppp PATH]
                         [--version] [-wcdf FILEPATH]
                         PDF_FILE [PDF_FILE ...]

对该目录输出的说明文本的翻译如下：

**描述：**
   
   这是一个用于裁剪 PDF 文件边距的命令行应用程序。裁剪边距可以使阅读 PDF 文档（无论是打印还是在屏幕上显示）变得更加容易，因为显示的字体会更大。当 PDF 文件作为图形包含在文档中时，边距裁剪有时也很有用。
   
   默认情况下，将保留现有边距的 10%，其余部分将被消除。但是，可以设置许多选项，包括要保留的现有边距的百分比。
   
   下面是一个简单的示例，说明如何裁剪名为 `document.pdf` 的文件，并将裁剪后的输出文档写入名为 `croppedDocument.pdf` 的文件：
   
      `pdfcropmargins document.pdf -o croppedDocument.pdf`
   
   也可以使用别名 `pdf-crop-margins` 来启动程序，代替 `pdfcropmargins`。如果没有提供目标文件，程序将根据源文件的名称自动生成一个文件名（见下文）。
   
   `pdfCropMargins` 程序通过更改 PDF 文件中存储的页面大小（并由像 Acrobat Reader 这样的程序解释）来工作。`CropBox` 和 `MediaBox` 都会被设置为新计算出的裁剪尺寸。之后，大多数程序中的文档视图将会是新的、裁剪后的视图。
   
   为了减少必须保存的文档副本数量，提供了一个基本的 `--restore` 选项。当裁剪一个不是由 `pdfCropMargins` 程序生成的文件时，默认情况下会将每个页面的 MediaBox 和任何现有 CropBox 的交集保存为 XML 元数据。这将保存原始文档的“通常”视图，以便在像 Acrobat Reader 这样的程序中查看。对由 `pdfCropMargins` 生成的文件进行后续裁剪时，默认情况下不会更改已保存的数据。恢复选项只是将保存的值复制回 MediaBox 和 CropBox。
   
   （旧版本的程序将数据保存到 ArtBox；如果这些文件再次被裁剪，则数据会迁移到 XML 元数据。）
   
   以下是使用更多命令行选项的几个示例，每个都应用于输入文件 `doc.pdf`。在大多数这些示例中，未指定输出文件名，因此程序将自动为输出生成文件名（或者始终可以通过 `-o` 明确提供输出文件名）。
   
   1) 将 `doc.pdf` 裁剪，使所有页面都设置为相同的大小，并且裁剪量在所有页面上均匀（这会产生漂亮的双栏外观）。使用默认的保留 10% 现有边距。请注意，`-u` 仅使要裁剪的量在每页上保持一致；如果页面最初大小不同，即使使用了 `-s` 选项，它们之后也不会具有相同的大小。
   
      `pdfcropmargins -u -s doc.pdf`
   
   2) 以 50% 的保留率独立裁剪 `doc.pdf` 的每一页。
   
      `pdfcropmargins -p 50 doc.pdf`
   
   3) 均匀裁剪 `doc.pdf`，保留 50% 的左边缘、20% 的底部边缘、40% 的右边缘和 10% 的顶部边缘。
   
      `pdfcropmargins -u -p4 50 20 40 10 doc.pdf`
   
   4) 裁剪 `doc.pdf`，保留 20% 的边距，然后绝对减少右侧页面边距 12 点。
   
      `pdfcropmargins -p 20 -a4 0 0 12 0 doc.pdf`
   
   5) 在所有页面的裸边界框周围添加一个常数 5bp（注意传递给 `-a` 选项的负值，这会增加空间而不是移除它）。
   
      `pdfcropmargins -p 0 -a -5 doc.pdf`
   
   6) 在计算边界框之前，预先裁剪文档 5bp 每侧。然后裁剪，保留 50% 的计算出的边距。这在处理扫描书籍等困难文档时很有用，这些文档的页面边缘有噪声或其他“特征”。
   
      `pdfcropmargins -ap 5 -p 50 doc.pdf`
   
   7) 裁剪 `doc.pdf`，重命名裁剪后的输出文件为 `doc.pdf`，并将原始文件备份为 `backup_doc.pdf`。
   
      `pdfcropmargins -mo -pf -su "backup" doc.pdf`
   
   8) 将 `doc.pdf` 的边距裁剪到其原始大小的 120%，增加边距。使用 Ghostscript 来查找边界框，而无需 `pdfCropMargins` 显式渲染。
   
      `pdfcropmargins -p 120 -c gb doc.pdf`
   
   9) 裁剪 `doc.pdf`，忽略整个文档中每条边上的最大 10 个边距（超过整个文档）。这对于所有页面都有非常相似边距的嘈杂文档特别好，或者当你想要忽略只出现在少数页面上的边缘注释时。
   
      `pdfcropmargins -m 10 doc.pdf`
   
   10) 裁剪 `doc.pdf`，启动 acroread 查看器以查看裁剪后的输出，然后查询是否应重命名裁剪文件 `doc.pdf` 并将原始文件备份为 `doc_uncropped.pdf`。
   
      `pdfcropmargins -mo -q doc.pdf`
   
   11) 裁剪 `doc.pdf` 的第 1-100 页，统一裁剪所有偶数页，统一裁剪所有奇数页。
   
      `pdfcropmargins -g 1-100 -e doc.pdf`
   
   12) 尝试将 `doc.pdf` 恢复到其原始边距，假设它是以前用 `pdfCropMargins` 裁剪的。请注意，即使它是恢复文件，默认输出文件名仍然是 `doc_cropped.pdf`。使用 `-mo` 选项可以修改 `doc.pdf` 并备份之前的版本。
   
      `pdfcropmargins -r doc.pdf`
   
   这个程序有许多不同的使用方法。一旦找到适用于特定任务或工作流程模式的方法，通常很方便创建一个简单的 shell 脚本（批处理文件），该脚本调用带有那些特定选项和设置的程序。Bash 和 Windows 的简单模板脚本随程序一起打包，在 bin 目录中。当在 Python 路径中可发现时，可以从用户的 Python 程序中调用该程序，使用如下代码：
   
      `from pdfCropMargins import crop`
   
      `crop(["-p", "20", "-u", "-s", "paper.pdf"])`
   
   当打印边距紧密裁剪的文档时，可能需要使用诸如“适合可打印区域”的选项。如果文本边缘被截断，可能还需要微调保留边距的大小。
   
   有时，PDF 文件损坏或不符合标准，以至于该程序的例程会引发错误并退出。在这种情况下，有时使用 Ghostscript 修复 PDF 文件会有所帮助。如果它能被 Ghostscript 读取，以下命令通常会足够修复它：
   
   `gs -o repaired.pdf -sDEVICE=pdfwrite -dPDFSETTINGS=/prepress corrupted.pdf`
   
   此命令还可用于将一些 PostScript (.ps) 文件转换为 PDF。选项 `--gsFix`（或 `-gsf`）将自动尝试应用此修复，前提是 Ghostscript 可用。有关该选项的更多信息，请参阅该选项的描述。
   
   `pdfCropMargins` 程序处理旋转页面（例如纵向与横向模式下的页面）的方式如下。所有旋转页面在读取后都会立即取消旋转。然后计算所有裁剪。最后，当裁剪应用于页面时，重新应用旋转。这可能会在混合不同旋转页面的文档中产生意外结果，尤其是在使用 `--uniform` 或 `--samePageSize` 选项时。对于旋转页面，所有接受四个参数的选项（每个参数对应一个边距）的参数都会移动，使得左、底、右、顶边距对应于屏幕外观（无论内部旋转如何）。
   
   所有 `pdfCropMargins` 的命令行选项都在下面进行了描述。以下定义有助于精确地定义某些选项的行为。 “delta 值” 是应用到每个原始页面以获得最终裁剪页面的绝对减少长度（以点为单位）。每个页面上每个边距都有一个 delta 值。在通常情况下，所有 delta 值都是正的（即所有边距大小都减小）。然而，delta 值可以是负的（例如，当 percentRetain > 100 或使用负绝对偏移时）。当 delta 值为负时，相应的边距大小将增加。
   
   **位置参数:**
   `PDF_FILE`
                  要裁剪的 PDF 文件的路径名。如果文件或目录名包含空格，请使用引号括起来。如果未通过 `-o` 标志提供裁剪 PDF 输出文件的文件名，则将生成一个默认输出文件名。默认情况下，它是与源文件名相同，但扩展名 ".pdf" 被替换为 "_cropped.pdf"，如果文件已存在则会覆盖。文件将在运行程序时的工作目录中写入。如果输入文件没有扩展名或扩展名不是 '.pdf' 或 '.PDF'，则会在现有（可能为空）扩展名后追加 '.pdf'。路径上执行通配符和 shell 变量展开。



**选项:**
   
   `-h, --help`
                  显示此帮助消息并退出。
   
   `-o OUTFILE_PATH_OR_DIR, --outfile OUTFILE_PATH_OR_DIR`
                  一个可选参数，指定裁剪输出文档应写入的目录或文件路径。如果未给出此选项，程序将从输入文件名生成输出文件名，并写入当前工作目录。如果只给出了一个目录，则生成的文件名将写入该目录中。默认情况下，字符串 "_cropped" 会附加到输入文件名上，就在文件扩展名之前。（如果扩展名不是 '.pdf' 或 '.PDF'，则也会将 '.pdf' 追加到扩展名上。）选项 '--usePrefix'、'--stringCropped' 和 '--stringSeparator' 可用于自定义生成的文件名。默认情况下，任何同名的现有文件都会被静默覆盖；这可以通过 '--noclobber' 选项避免。对目录路径执行通配符和 shell 变量展开，但不对文件名部分执行。输出文件路径不能与输入文档路径相同（请改用 '--modifyOriginal' 选项）。
   
   `-v, --verbose`
                  打印有关程序操作和进度的更多信息。如果不带此开关，仅向屏幕打印警告和错误消息。
   
   `-gui, --gui`
                  运行图形用户界面。此模式允许您交互式预览和测试不同的裁剪选项，而无需每次都重新计算边界框（这可能很慢）。程序的所有常规命令行选项仍然受尊重。点击 GUI 中的“裁剪”按钮会使用当前设置进行裁剪，并将裁剪后的 PDF 文件写入与命令行版本将写入的相同文件名。GUI 中对边距的连续更改不是累积的：设置总是应用于传递给程序的原始文档。'原始'按钮将显示恢复到原始版本。
   
   `-p PCT, --percentRetain PCT`
                  设置图像中保留的边距空间的百分比。这是一个原始边距空间的百分比。默认情况下，百分比值设置为 10。将百分比设置为 0 会得到一个紧密的边界框。百分比值大于 100 会增加边距大小，负值会使边距减少得比紧边界框更少。
   
   `-p4 PCT PCT PCT PCT, -pppp PCT PCT PCT PCT, --percentRetain4 PCT PCT PCT PCT`
                  分别设置左、底、右、顶边距保留的边距空间百分比。四个参数应该是百分比值。百分比值大于 100 会增加边距大小，负值会使边距减少得比紧边界框更少。
   
   `-pt, --percentText`
                  通常，传递给 `--percentRetain` 或 `--percentRetain4` 的百分比值定义了要保留的现有边距的百分比。此标志改变了这些百分比值的解释。边距改为设置为边界框宽度的给定百分比（左右边距）和边界框高度的给定百分比（底边距和顶边距）。
   
   `-a BP, --absoluteOffset BP`
                  在应用 `percentRetain` 选项后，从每个边距的大小中减去一个绝对浮点偏移值，以从每个边距的大小中减去。单位是大点 (bp)，这是 PDF 文件中使用的单位。1 英寸有 72 bp。1 个 bp 大约等于 TeX 点 (pt)（1 英寸有 72.27 pt）。允许负值；正值总是减少边距大小，负值总是增加它。绝对偏移总是在任何百分比变化操作之后应用。
   
   `-a4 BP BP BP BP, -aaaa BP BP BP BP, --absoluteOffset4 BP BP BP BP`
                  分别用四个绝对偏移值减小边距大小。四个浮点参数应分别为左、底、右、顶偏移值。请参阅 `--absoluteOffset` 选项以获取有关单位的信息。
   
   `-cs, --cropSafe`
                  保证所有裁剪都是安全的，即没有任何裁剪会超出任何边距的紧边界框。这不适用于使用 `--absolutePreCrop` 选项的预裁剪。它也不适用于由于 `--uniformOrderStat` 或 `--uniformOrderStat4` 选项而忽略的页面上的任何边距。在 GUI 中，这与统一裁剪配合良好：如果没有任何有用文本会被裁剪出来，可以将 `uniformOrderStat` 的值增加到最小 delta 值所在的边距按钮上。`--cropSafeMin` 选项允许修改最小边距值，向边界框添加。
   
   `-csm4 BP BP BP BP, --cropSafeMin4 BP BP BP BP`
                  `--cropSafe` 选项不会执行任何切割进入边界框的裁剪。此选项修改了该选项的行为（假定也选择了 `--cropSafe`）。相反，它不会裁剪超过边界框加上相应边距值的范围。这适用于所有边距。该选项接受四个浮点数，单位为大点，分别为左、底、右、顶边距。允许负值，并允许裁剪部分边界框。
   
   `-ap BP, --absolutePreCrop BP`
                  此选项类似于 `--absoluteOffset`，但它是应用在任何边界框计算（或任何其他操作）之前。参数相同，单位为 bp。所有后续操作都相对于此预裁剪框进行，被认为是全页框。请注意，由于此绝对裁剪是在计算任何边界框之前应用的，因此它是相对于文档的原始全页框（不同于 'absoluteOffset'，后者是相对于 'percentRetain' 应用后的新裁剪边距而言的）。因此，所需的点数可能需要比 'absoluteOffset' 更大。此选项可用于在计算边界框之前忽略文本和标记物，从而在边缘裁剪掉它们。
   
   `-ap4 BP BP BP BP, --absolutePreCrop4 BP BP BP BP`
                  与 `--absolutePreCrop` 相同，但可以分别给出四个参数。四个浮点参数应分别为左、底、右、顶绝对预裁剪值。
   
   `-u, --uniform`
                  统一裁剪所有页面。这强制每个页面的边距裁剪（绝对，而非相对）量相同。此选项在为每个页面单独计算所有 delta 值后应用。然后，所有页面的左边距 delta 值都设置为所有页面中最小的左边距 delta 值。底、右、顶边距也是类似处理。请注意，这实际上会为某些页面增加一些边距空间（相对于单独裁剪页面获得的边距）。如果原始文档的页面都是相同大小，那么裁剪后的页面也将全部相同大小。可以与 `--samePageSize` 选项结合使用，以强制所有页面在裁剪后大小相同。
   
   `-u4 {t,f} {t,f} {t,f} {t,f}, --uniform4 {t,f} {t,f} {t,f} {t,f}`
                  此选项与 `--uniform` 相同，但仅应用于选定的边距。四个参数应为字符 't' 或 'f'，分别选择（t）或取消选择（f）左、底、右、顶边距。
   
   `-m INT, --uniformOrderStat INT`
                  选择此选项意味着启用 `--uniform` 选项，但不再选择所有页面中最小的 delta 值。相反，对于每个边距，选择所有页面中第 n 个最小的 delta 值（n 从零开始编号）。参数是整数 n，例如 '-m 4'。此选项对于裁剪嘈杂的扫描 PDF 很有用，这些 PDF 在大多数页面上有一个共同的边距大小，或者用于忽略只出现在少数页面上的注释。此选项本质上导致程序在计算所有页面的公共 delta 值时忽略最大的 n 个紧裁剪边距。增加 n 总是增加裁剪量或保持不变。可能需要一些试验和错误来选择最佳数字。使用 '-m 1' 与 arXiv 论文（第一页的边距中有日期）配合得很好。
   
   `-m4 INT INT INT INT, -mmmm INT INT INT INT, --uniformOrderStat4 INT INT INT INT`
                  此选项与 `--uniformOrderStat`（或 `-m`）相同，但为每个边距单独指定值。边距按左、底、右、顶顺序排列。
   
   `-mp INT, --uniformOrderPercent INT`
                  此选项与 `--uniformOrderStat` 相同，但排序数 n 自动设置为要裁剪的页面数的给定百分比（无论是总数还是用 `--pages` 设置的页面）。此选项覆盖 `--uniformOrderStat` 如果两者都设置了。参数是浮点百分比值；四舍五入以获得最终的排序数。将百分比设置为 0 相当于 n=1，将百分比设置为 100 相当于将 n 设置为页面总数，将百分比设置为 50 则给出中位数（对于奇数个页面）。
   
   `-s, --samePageSize`
                  设置所有页面大小相等。此选项仅在页面大小不同时才有效。页面大小设置为所有页面区域的联合，即包含所有页面的最小边界框。此操作总是先于其他操作（除了 `--absolutePreCrop`）完成。然后照常进行裁剪，但请注意，任何边距百分比（如 `--percentRetain`）现在都是相对于这个新的、可能更大的页面大小。结果页面仍然是独立裁剪的，默认情况下不会都具有相同的大小，除非也选择了 `--uniform` 以强制每个页面的裁剪量相同。如果用 `--pages` 选择了页面，则此选项仅应用于这些选定的页面。
   
   `-s4 {t,f} {t,f} {t,f} {t,f}, --samePageSize4 {t,f} {t,f} {t,f} {t,f}`
                  此选项与 `--samePageSize` 相同，但仅应用于选定的边距。四个参数应为字符 't' 或 'f'，分别选择（t）或取消选择（f）左、底、右、顶边距。
   
   `-ms INT, --samePageSizeOrderStat INT`
                  选择此选项意味着启用 `--samePageSize` 选项，但计算每个边的最小边界框时忽略最大的（或左和底边的最小）n 个值。参数是非负数 n。每个边都独立计算。这是选择统一大小以制作页面的顺序统计。请注意，如果 n>0，这会切掉某些页面的部分。
   
   `-ssp FLOAT FLOAT FLOAT FLOAT, --setSamePageSize FLOAT FLOAT FLOAT FLOAT`
                  此选项类似于 `--samePageSize` 选项，但传入的页面大小是作为四个浮点参数而不是计算得出的。数字应代表左、底、右、顶边距值，分别为。原点位于左下角。数字应以点为单位，并且是绝对的，而不是相对于任何当前边距。`--samePageSize` 选项将覆盖此选项，如果它被设置。
   
   `-e, --evenodd`
                  统一裁剪所有奇数页，统一裁剪所有偶数页。选择适用于所有页面的最大裁剪量。如果同时设置了 `--uniform` ('-u') 选项，则垂直裁剪将在所有页面上均匀，只有水平裁剪在偶数和奇数页之间有所不同。另请参见 `--percentText` 选项，可用于类似效果。
   
   `-g PAGESTR, -pg PAGESTR, --pages PAGESTR`
                  仅对选定的页面应用裁剪操作。参数应为通常形式的列表，例如 "2-4,5,9,20-30"。页码编号假定从 1 开始。参数列表中的顺序无关紧要，负范围被忽略，超出文档的页面被忽略。请注意，恢复信息总是为所有页面保存（在 ArtBox 中），除非选择了 `--noundosave`。
   
   `-c [d|m|p|gr|gb|o], --calcbb [d|m|p|gr|gb|o]`
                  选择计算边界框（或渲染 PDF 页面以计算框）的方法。默认选项 'd' 当前会选择 MuPDF 渲染选项。强制使用特定方法的选项是 MuPDF ('m')，pdftoppm ('p') 或 Ghostscript ('gr') 用于渲染，或直接 Ghostscript 边界框计算 ('gb')。对于 pdftoppm 或 Ghostscript 选项，相应的程序必须安装并且可定位（如果默认定位器失败，请参阅路径设置选项）。只有显式渲染方法才能用于扫描页面（请参阅 `--gsBbox`）。选择 'o' 会恢复到旧的默认行为，首先查找 pdftoppm，然后查找 Ghostscript 用于渲染。
   
   `-gs, --gsBbox`
                  此选项是为了向后兼容性维护；现在建议使用 `-c gb`。使用 Ghostscript 直接查找页面的边界框，而无需显式渲染页面。 （默认是显式渲染 PDF 页面为图像文件，并从图像中计算边界框。）这种方法通常要快得多，但它不适用于扫描的 PDF 文档。它也无法选择阈值值，应用模糊等。任何分辨率选项都将传递给 Ghostscript bbox 设备。此选项要求 Ghostscript 在 PATH 中可用，Windows 上为 "gswin32c.exe" 或 "gswin64c.exe"，Linux 上为 "gs"。当设置此选项时，Python 的 Pillow 图像库将不需要。
   
   `-gsr, --gsRender`
                  此选项是为了向后兼容性维护；现在建议使用 `-c gr`。使用 Ghostscript 将 PDF 页面渲染为图像。（默认情况下，PyMuPDF 程序将优先用于渲染。）请注意，此选项在选择 `--gsBbox` 时无效，因为那时不会进行显式渲染。
   
   `-t BYTEVAL, --threshold BYTEVAL`
                  设置确定什么是背景空间（白色）的阈值。该值可以从 0 到 255，191 是默认值（75%）。此选项可能在某些配置中不可用，因为 PDF 必须在内部渲染为像素图像。特别是，当 `--gsBbox` 被选择时，它被忽略。任何高于阈值的像素值被认为是背景（白色），任何低于它的值被认为是文本（黑色）。降低该值应该倾向于使边界框变小。例如，对于灰色背景的扫描图像，可能需要降低阈值。对于深色背景和浅色文本的页面，可以使用负阈值。在这种情况下，绝对值用作阈值，但测试被反转，考虑像素值大于或等于阈值为背景。
   
   `-nb INT, --numBlurs INT`
                  当 PDF 文件被显式渲染为图像文件时，对该结果图像应用模糊操作多次。这对于噪声图像很有用。
   
   `-ns INT, --numSmooths INT`
                  当 PDF 文件被显式渲染为图像文件时，对该结果图像应用平滑操作多次。这对于噪声图像很有用。
   
   `-x DPI, --resX DPI`
                  在渲染图像以查找边界框时使用的 x 方向分辨率（每英寸点数）。默认为 150。较高的值会产生更精确的边界框，但需要更多时间和内存。
   
   `-y DPI, --resY DPI`
                  在渲染图像以查找边界框时使用的 y 方向分辨率（每英寸点数）。默认为 150。较高的值会产生更精确的边界框，但需要更多时间和内存。
   
   `-sr STR, --screenRes STR`
                  传递一个 X-windows 风格的几何字符串，供 GUI 用作全屏分辨率以及窗口的左上角位置。这主要是当屏幕大小检测算法对特定系统失败时使用。例如，对于大小为 "1024x720" 的屏幕，该字符串应该使用该选项。要将窗口放置在 (0,0)，字符串将是 "1024x728+0+0"。另请参阅 `--guiFontSize` 选项，该选项可用于减小 GUI 窗口的整体大小。
   
   `-gf INT, --guiFontSize INT`
                  选择 GUI 字体大小。将其设置为小于默认值 11 的值也可以使 GUI 变小，如果它不适合较小的显示器。
   
   `-b [m|c|t|a|b], --boxesToSet [m|c|t|a|b]`
                  默认情况下，pdfCropMargins 程序将每个页面的 MediaBox 设置为新裁剪的页面大小。此默认设置通常是足够的，但此选项可用于选择不同的 PDF 箱来设置。该选项采用一个参数，即箱类型的第一个字母（小写）。选择有 MediaBox (m), CropBox (c), TrimBox (t), ArtBox (a), 和 BleedBox (b)。此选项覆盖默认值，并可以重复多次以设置多个箱类型。请注意，程序现在使用 PyMuPDF 来设置箱子，并且除非它们完全包含在 MediaBox 内，否则它将拒绝设置任何非 MediaBox 箱。在这种情况下，会发出警告并且箱子不会被设置。
   
   `-f [m|c|t|a|b], --fullPageBox [m|c|t|a|b]`
                  默认情况下，程序首先（在任何裁剪计算之前）将每个页面的 MediaBox 设置为该页面的先前 MediaBox 和 CropBox 的交集。这确保了裁剪相对于 Acrobat Reader 等程序中的通常文档视图进行。这本质上定义了文档中假设的完整页面大小，所有裁剪都相对于该完整页面大小进行。此选项可用于交替使用 MediaBox、CropBox、TrimBox、ArtBox 或 BleedBox 来定义完整页面大小。该选项采用一个参数，即箱类型的第一个字母（小写）。如果重复该选项，则使用所有箱参数的交集。只有一个选择与 `-gs` 选项组合，因为 Ghostscript 在查找边界框时会进行自己的内部渲染。使用 `-gs` 时的默认值是 CropBox。
   
   `-r, --restore`
                  这是一个简单的撤销操作，基本上会撤销 `pdfCropMargins` 所做的所有裁剪，并返回到原始边距（假设没有其他程序修改了 `pdfCropMargins` 键的保存数据）。默认情况下，每当该程序第一次裁剪一个文件时，它会将每个页面的 MediaBox 与任何现有 CropBox 的交集保存为 XML 元数据。检查是否有任何现有的恢复数据。如果有，只需将保存的元数据复制到每个页面的 MediaBox 和 CropBox。这将恢复早期的文档视图，例如在 Acrobat Reader 中（但在 MediaBox 和 CropBox 不同的情况下，不会完全恢复之前的状态）。任何不适用于恢复操作的选项，如 '-u', '-p', 和 '-a'，都会被忽略。请注意，就默认文件名而言，此操作被视为另一个裁剪操作（默认生成的输出文件名仍然带有 "_cropped.pdf" 后缀）。可以使用 `--modifyOriginal` 选项（或其查询变体）与此选项一起使用。保存恢复数据为 XML 元数据可以通过使用 `--noundosave` 选项来禁用。
   
   `-A, --noundosave`
                  不保存任何恢复数据为 XML 元数据。请注意，如果此选项包含在裁剪命令中，则以后无法正确地对裁剪后的文档执行恢复操作。
   
   `-gsf, --gsFix`
                  尝试在读取之前用 Ghostscript 修复输入 PDF 文件。这需要 Ghostscript 可用。（请参阅上面的通用描述文本以了解实际运行的命令。）这也可以用于自动将一些 PostScript 文件 (.ps) 转换为 PDF 以进行裁剪。修复后的 PDF 会写入临时文件；原始 PDF 文件不会被修改。原始文件名将像往常一样处理，用于自动名称生成、`--modifyOriginal` 选项以及诸如此类的操作。如果程序因损坏的 PDF 文件而挂起或引发错误，此选项通常会有帮助。请注意，当重新裁剪一个已经由 pdfCropMargins 裁剪的文件时，此选项可能不需要，并且如果在重新裁剪时使用（至少在当前版本的 Ghostscript 中），它会重置 pdfCropMargins 程序用来判断文件是否已由程序裁剪的 Producer 元数据（所以 `--restore` 选项不能与此选项一起使用）。除非遇到许多损坏的 PDF 文件并且不需要恢复到原始边距，否则不建议默认使用此选项。
   
   `-nc, --noclobber`
                  永远不要覆盖现有的文件作为裁剪输出文件。
   
   `-pv PROG, --preview PROG`
                  运行 PDF 查看器以查看裁剪后的 PDF 输出。查看器进程在后台运行。在 pdfCropMargins 完成所有其他选项后，运行查看器。唯一的例外是当同时选择了 `--queryModifyOriginal` 选项时。在这种情况下，查看器在查询之前启动，以便用户可以在决定是否修改原始文件之前查看输出。（请注意，回答 'y' 将把文件从正在运行的查看器下移走；在添加注释、高亮显示等之前关闭并重新打开文件。）单个参数应为可执行文件或脚本的路径。查看器假定恰好有一个参数，即 PDF 文件名。例如，在 Linux 上，可以使用 /usr/bin/acroread 选择 Acrobat Reader，或者如果它在 PATH 中，则只需 acroread。可以使用 shell 脚本或批处理文件包装器来设置查看器的任何附加选项。
   
   `-mo, --modifyOriginal`
                  此选项将原始文档文件移动（重命名）到备份文件名，然后将裁剪文件移动到原始文档的文件名（和目录路径）。因此，它有效地在原地裁剪原始文档文件，并将原始文件的备份副本写入输出目录。原始文件的备份文件名始终从原始文件名生成；任何将由程序生成的前缀或后缀（默认为 "_cropped"）都会相应地修改（默认为 "_uncropped"）。可以使用 `--usePrefix`, `--stringUncropped` 和 `--stringSeparator` 选项来自定义生成的备份文件名。如果通过 `--outfile` ('-o') 选项指定了输出路径，则备份文档将写入该路径（如果只提供了文件名，则写入裁剪文件首次写入的同一目录中）。此操作最后执行，因此如果之前的任何操作失败，原始文档将保持不变。请注意，使用 pdfCropMargins 两次在同一源路径上使用此选项将修改已备份的原始文件；`--noclobberOriginal` 选项可用于避免这种情况。
   
   `-q, --queryModifyOriginal`
                  此选项选择 `--modifyOriginal` 选项，但会询问用户是否实际执行最终的移动操作。这与 `--preview` 和/或 `--gui` 选项配合得很好：如果预览看起来不错，您可以选择修改原始文件（保留原始文件的副本）。如果您拒绝，则文件不会交换（并且就像没有选择 `--modifyOriginal` 选项一样）。
   
   `-ro, --replaceOriginal`
                  此选项意味着 `--modifyOriginal` 选项，并且工作方式相同，但不会创建备份副本。原始文件被删除，裁剪文件被移动到原始文件名。此选项可以与 `--queryModifyOriginal` 结合使用，工作方式相同，但原始文件被替换，没有备份副本。
   
   `-nco, --noclobberOriginal`
                  如果选择了 `--modifyOriginal` 选项，则永远不要覆盖现有的文件作为原始文件的备份副本。这本质上是在 noclobber 模式下执行 `--modifyOriginal` 选项的移动操作，并在失败时发出警告。失败的结果正好就像没有选择 `--modifyOriginal` 选项一样。如果还设置了普通的 `--noclobber` 选项，则此选项是多余的。
   
   `-pf, --usePrefix`
                  在生成默认文件名时，使用前缀字符串而不是后缀字符串。使用相同的字符串值，无论是默认值还是通过 `--stringCropped` 或 `--stringUncropped` 选项设置的值。在默认值和其他选项无输出文件指定的情况下，此选项会导致裁剪输出的输入文件 "document.pdf" 写入文件 "cropped_document.pdf"（而不是默认文件名 "document_cropped.pdf"）。
   
   `-sc STR, --stringCropped STR`
                  此选项可用于设置在自动为裁剪文件生成输出文件名时将附加（或前置）到文档文件名的字符串。默认值是 "cropped"。
   
   `-su STR, --stringUncropped STR`
                  此选项可用于设置在自动为原始、未裁剪文件生成输出文件名时将附加（或前置）到文档文件名的字符串。默认值是 "uncropped"。
   
   `-ss STR, --stringSeparator STR`
                  此选项可用于设置在附加或前置字符串值以自动生成文件名时使用的分隔符字符串。默认值是 "_"。
   
   `-pw PASSWD, --password PASSWD`
                  指定用于解密加密 PDF 文件的密码。请注意，总是尝试使用空密码进行解密，因此此选项仅用于非空密码。生成的裁剪文件将不会被加密，因此在涉及重要数据时请谨慎。
   
   `-pc, --prevCropped`
                  测试文档是否以前由 pdfCropMargins 程序裁剪过。如果是，退出代码为 0。如果不是，退出代码为 1。此选项主要用于脚本编写，例如，仅裁剪尚未被裁剪过的文档。当选择此选项时，只有 '--gsFix', '--version' 和 '--help' 选项被接受。
   
   `-khc, --keepHorizCenter`
                  保持 PDF 的水平中心点固定。通常的裁剪是计算出来的，但对于每一页，左右 delta 值都设置为两个值中的最小值（因此两侧的裁剪量相同）。此选项不适用于预裁剪。
   
   `-kvc, --keepVertCenter`
                  保持 PDF 的垂直中心点固定。通常的裁剪是计算出来的，但对于每一页，上下 delta 值都设置为两个值中的最小值（因此顶部和底部的裁剪量相同）。此选项不适用于预裁剪。
   
   `-spr FLOAT:FLOAT, --setPageRatios FLOAT:FLOAT`
                  强制所有裁剪页面的比例等于给定比例。所有裁剪都按通常方式进行计算和应用，但为了使宽高比等于目标值，左右边距将同等增加，或者上下边距将同等增加。边距只会增加。比例的格式可以是字符串宽度-高度比例，例如 '4.5:3'，或者浮点数，例如 '0.75'，这是宽度除以高度。此选项在某些 PDF 查看器中可能有用。
   
   `-prw FLOAT FLOAT FLOAT FLOAT, --pageRatioWeights FLOAT FLOAT FLOAT FLOAT`
                  此选项加权 `--setPageRatios` 参数添加的任何空白。它有四个权重参数，每个边距一个。四个浮点参数应分别为左、底、右、顶权重。所有权重必须大于零。
   
   `-ct, --centerText`
                  在裁剪后水平和垂直居中文本。调整每页的裁剪，使紧边界框居中于页面（如果可能）。如果应用了像 `--uniformOrderStat` 这样的顺序统计方法，对于被忽略的边，用于计算裁剪值的边界框边将被使用。如果设置了 `--centeringStrict` 标志，则无论任何顺序统计计算，每个页面都将被居中。
   
   `-ch, --centerTextHoriz`
                  与 `--centerText` 相同，但页面仅水平居中。
   
   `-cv, --centerTextVert`
                  与 `--centerText` 相同，但页面仅垂直居中。
   
   `-cst, --centeringStrict`
                  此标志修改了像 `--centerText` 这样的边界框居中选项的行为。通常，用于顺序统计操作（如 `--uniformOrderStat`）忽略的页面也用于居中，使用用于裁剪的实际页面进行居中。此选项强制严格居中每个页面。
   
   `-i, --showImages`
                  当显式将 PDF 文件渲染为图像文件时，显示用于查找边界框的反向图像文件。对于调试和选择其他参数（如阈值）很有用。此选项需要由 Pillow 图像操作包选择的默认外部查看器程序（Unix 上的 xv，Windows 上通常是 Paint）。
   
   `-gsp PATH, --ghostscriptPath PATH`
                  传递 Ghostscript 可执行文件的路径，程序应使用它。不进行通配符展开。当程序位于非标准位置时很有用。
   
   `-ppp PATH, --pdftoppmPath PATH`
                  传递 pdftoppm 可执行文件的路径，程序应使用它。不进行通配符展开。当程序位于非标准位置时很有用。
   
   `--version`
                  返回 pdfCropMargins 版本号并立即退出。所有其他选项都被忽略。
   
   `-wcdf FILEPATH, --writeCropDataToFile FILEPATH`
                  将计算出的裁剪列表写入传入的文件路径并退出。主要用于自动化测试和调试。


   pdfCropMargins程序的版权归Allen Barker所有(c)2014。

   根据GNU GPL许可证发布，版本3或更高版本。

