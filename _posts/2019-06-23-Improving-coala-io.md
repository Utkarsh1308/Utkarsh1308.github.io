---
layout: post
title: Improving coala io!!
subtitle: The real work begins.
bigimg: /img/path.jpg
---

With the start of the second half of Phase-1 I shifted my focus to the coala codebase. Before adding support for **Binary and XML Diffs**
I was tasked with improving the existing codebase. This included adding context for diffs and adding support for handling files with 
non UTF-8 charset.

## Add setting charset

coala has a File class for giving different views on a file such as the modifcation timestamp, the file in raw format, The file in the 
form of a string UTF-8 decoded from the raw format. 

During processing, coala calls **get_file_dict** method which calls the **File.string** method.
This is to purposely raise a **UnicodeDecodeError** when coala tries to open files with an encoding different than UTF-8. The string
method tries to decode the raw contents of the file with encoding set to UTF-8.

~~~
    def string(self):
        """
        :return:
            The file contents as a string UTF-8 decoded.
        """
        return self.raw.decode(encoding='utf-8')
~~~

This gives an error in case of files with different encodings. In case of a UTF-16 encoded file we get the following error:

~~~
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 0: invalid start byte
~~~

To add support for these files I made use of the **detect_encoding** method from **FileUtils** in **coala-utils**.
You will notice that for UTF-16 files, there are 2 bytes at the front that are unique to UTF-16. These are called 
Byte Order Markers (BOM). The **detect_encoding** detects this to guess that the file is meant to be decode using
UTF-16.

I modified the **File.String** Method to allow the handling of files with charset different from UTF-8.

~~~
return self.raw.decode(encoding=detect_encoding(self._filename))
~~~

This allowed coala to handle files with UTF-8, UTF-16, UTF-32 encoding. 

After fixing this problem. I wrote some end to end tests to break coala to find problems. These are some problems which I found 

* LineCountBear returns the wrong number of lines for utf-32 encoded files
* When a file is written in python using 

~~~
with open(file, 'w') as fd:
    fd.write('Capit\xe1n ')
~~~

And coala tries to open it. The file opens fine on linux but gives a **UnicodeDecodeError** on Windows.
This is because of a difference in the default encoding in which files are saved in the 2 platforms.
In linux files are encoded in utf-8 by default and on windows they are encoded in cp1252 encoding.

## Context For Diffs

There has been a lot of discussion in closing this [issue](https://github.com/coala/coala/issues/2745) including 2 PRs.
Both the PRs are stale as of yet. With no sight of this issue being taken by anyone anytime soon I decided to take it up 
myself.

I thought of an enhancement to the approaches suggested by these PRs. They add a feature to improve diffs by adding context for 
the line of code. Their version of the context includes checking indentation level and printing the lines before the 
current line with less indentation. In case of no indentation it prints the 2 lines above the line of code. 

I thought of improving this further by printing only the parents (function, class) to which the line belongs to. The following is the 
code for getting the context 

~~~
def get_context(console_printer, file_dict,
                sourcerange):
    """
    Gets context for the affected lines and also the line numbers
    of the context

    :param console_printer: Object to print messages on the console.
    :param file_dict:       A dictionary containing all files as values with
                            filenames as key.
    :param sourcerange:     The SourceRange object referring to the related
                            lines to print.
    :return:                Affected file's contextual information.
    """

    related_lines = list()
    related_lines_number = list()

    affected_line = file_dict[sourcerange.file][
                    sourcerange.start.line-1].rstrip().replace('\t', 8*' ')

    indent_space_count = len(affected_line)-len(affected_line.lstrip())
    if indent_space_count > 0:
        for line in range(sourcerange.start.line-2, -1, -1):  # pragma: no cover
            rline = file_dict[sourcerange.file][
                    line].rstrip().replace('\t', 8*' ')
            previous_space_count = len(rline) - len(rline.lstrip())
            for language in PARENTS:
                if any(keyword in rline for keyword in language[1:]):
                    if previous_space_count < indent_space_count and rline:
                        related_lines.append(rline)
                        related_lines_number.append(line+1)
                        indent_space_count = previous_space_count
                    if indent_space_count == 0:
                        break
    else:
        pre_context = list(range(sourcerange.start.line-1))
        pre_context.reverse()
        pre_context = pre_context[0: min(2, len(pre_context))]
        for line in pre_context:
            rline = file_dict[sourcerange.file][
                    line].rstrip().replace('\t', 8*' ')
            previous_space_count = len(rline) - len(rline.lstrip())
            if previous_space_count <= indent_space_count and rline:
                related_lines.append(file_dict[sourcerange.file][line])
                related_lines_number.append(line+1)

    related_lines_number.reverse()
    related_lines.reverse()

    return related_lines, related_lines_number
~~~

**PARENTS** is a data structure which contains the parent keywords for the languages which coala supports.

## Phase-1 Ends

Wait, this month already ended? The world cup is already halfway through O___O

There is still more work to be done in the coming phases. I'll go write my evaluation now.

