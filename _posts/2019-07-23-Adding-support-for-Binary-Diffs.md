---
layout: post
title: Tackling Binary code!!
subtitle: Adding support for Binary Diffs to coala
bigimg: /img/path.jpg
---

There are different functions dedicated to printing each line of code in the coala output including the information regarding the bear, 
file contents and the context. The diffs are the part which I am interested in. **ShowPatchAction** has a method **print_beautified_diff** 
which prints the diff to the console. 

## Handling binary code in Diff objects

The methods in the Diff class were written taking into consideration a unified diff system. For adding a binary diff support along with 
that we need a new function **binary_diff()** which stores the binary diff for the patch. 

~~~
@property
    def binary_diff(self, filename, output_filename):
        """
        Generates a binary diff corresponding to this patch.

        The diff will display the byte addresses where the changes have taken
        place. The whole diff will be printed at once.
        """

        self._changes['diff'] = main([filename, output_filename, '--diff',
                                    '--bytes', '20'])
~~~

We use the **main()** method from multidiff to generate the binary diffs. Here filename belongs to the original file and output_filename
belongs to the new binary file after performing the operations. (The reduced size image in case of ImageCompressionBear).

## Printing Binary Diffs 

After generating the binary Diffs we need to create a new function **print_binary_diff()** in **ShowPatchAction** to print the binary 
diffs to console

~~~
def print_binary_diff(difflines, filename, to_filename, printer):
    print_from_name(printer, filename)
    print_to_name(printer, to_filename)

    for line_nr, line in enumerate(difflines.split('\n'), 1):
        printer.print('\n'.join('[{:>4}] {}'.format(line_nr, line)))
~~~

We also need to change the **ShowPatchAction.apply()** method:

~~~
def apply(self,
              result,
              original_file_dict,
              file_diff_dict,
              no_color: bool = False,
              show_result_on_top: bool = False):
        """
        (S)how patch

        :param no_color:
            Whether or not to use colored output.
        :param show_result_on_top:
            Set this to True if you want to show the result info on top.
            (Useful for e.g. coala_ci.)
        """
        printer = ConsolePrinter(not no_color)

        if show_result_on_top:
            from coalib.output.ConsoleInteraction import print_result
            # Most of the params are empty because they're unneeded in
            # noninteractive mode. Yes, this cries for a refactoring...
            print_result(printer, None, {}, result, {}, interactive=False)

        for filename, this_diff in sorted(result.diffs.items()):
            to_filename = this_diff.rename if this_diff.rename else filename
            to_filename = '/dev/null' if this_diff.delete else to_filename
            original_file = original_file_dict[filename]
            if result.additional_info != 'binary':
                try:
                    current_file = file_diff_dict[filename].modified
                    new_file = (file_diff_dict[filename] + this_diff).modified
                except KeyError:
                    current_file = original_file
                    new_file = this_diff.modified

                if tuple(current_file) != tuple(new_file):
                    print_beautified_diff(difflib.unified_diff(current_file,
                                                               new_file,
                                                               fromfile=filename,
                                                               tofile=to_filename),
                                          printer)
                elif filename != to_filename:
                    print_from_name(printer, join('a', relpath(filename)))
                    print_to_name(printer, join('b', relpath(to_filename)))
            else:
                print_binary_diff(this_diff._changes['diff'],
                                  filename,
                                  to_filename,
                                  printer)
        return file_diff_dict
~~~

We add **binary** as **additional_info** to the result and SourceRange objects for coala to know that it is dealing with binary code.


The binary diffs look like below

~~~
main.c
**** ImageCompressionBear [Section: cli | Severity: NORMAL] ****
!    ! This Image can be Losslessly compressed To 4652 bytes (savings: 1348 bytes = 22.46%)
[----] c:\users\utkarsh\main.c
[++++] c:\users\utkarsh\main.c
<-- BINARY DIFF HERE -->
[    ] *0. Do (N)othing
[    ]  1. (O)pen file
[    ]  2. (A)pply patch
[    ]  3. Print (M)ore info
[    ]  4. Add (I)gnore comment
[    ]  5. Show Applied (P)atches
[    ]  6. (G)enerate patches
[    ] Enter number (Ctrl-Z to exit):
~~~

## End of Phase-2

Now that we have a clear approach on how to implement Binary Diffs in coala code we can shift our focus on XML Diffs.
With my college starting in a week I plan to complete most of my work in the coming couple weeks but we never know what the future will await.

Hoping for the best!!
