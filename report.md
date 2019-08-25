---
layout: report
student: Utkarsh Sharma
organisation: coala
organisation_link : https://coala.io
project: Improve Diff Handling
project_link: https://summerofcode.withgoogle.com/projects/#6443787042160640
tarball: https://github.com/Utkarsh1308/Utkarsh1308.github.io/blob/master/Utkarsh1308_coala.tar.gz?raw=true
mentors: >
 [John Vandenberg](https://github.com/jayvdb)
phase:
 - Bonding : https://gitlab.com/coala/GSoC/gsoc-2019/-/milestones/26
 - Phase 1 : https://gitlab.com/coala/GSoC/gsoc-2019/-/milestones/29
 - Phase 2 : https://gitlab.com/coala/GSoC/gsoc-2019/-/milestones/30
 - Phase 3 : https://gitlab.com/coala/GSoC/gsoc-2019/-/milestones/31
bio: >
 I'm a fourth year student (expected graduation date: May 2021) at Birla Institute of Technology and Science, Goa. I participated in GSoC and worked with [coala](http://coala.io) on improving Diff Handling, by improving the context for the affected code shown by diffs, and added support for non utf-8 encodings in coala. I also added support for Binary Diffs and XML Diffs.
social:
 - GitHub:
   - username: Utkarsh1308
   - link: https://github.com/Utkarsh1308
 - GitLab:
   - username: Utkarsh1308
   - link: https://gitlab.com/Utkarsh1308
 - Gitter:
   - username: Utkarsh1308
   - link: https://gitter.im/Utkarsh1308
 - LinkedIn:
   - username: Utkarsh Sharma
   - link: https://www.linkedin.com/in/utkarsh-sharma-375159178/
email: usharma1308@gmail.com
blog: https://utkarsh1308.github.io/
activity:
 - 0:
   - repo: cEPs
   - link: https://github.com/coala/cEPs/pull/190
   - details: >
     cEP-0034: Improve Diff Handling
 - 1:
   - repo: multidiff
   - link: https://github.com/juhakivekas/multidiff/commit/8d2cd0f0f119932236fe2826891f1a2d4df7d5a8
   - details: >
      Add .travis.yml
 - 2:
   - repo: multidiff
   - link: https://github.com/juhakivekas/multidiff/pull/8
   - details: >
      Add width and bytes attribute
 - 3:
   - repo: multidiff
   - link: https://github.com/juhakivekas/multidiff/pull/9
   - details: >
      Add diff attribute
 - 4:
   - repo: coala-utils
   - link: https://gitlab.com/coala/coala-utils/merge_requests/99
   - details: >
      FileUtils.py: Fix detect_encoding
 - 5:
   - repo: coala-utils
   - link: https://gitlab.com/coala/coala-utils/merge_requests/100
   - details: >
      Add support for non utf-8 encodings
 - 6:
   - repo: coala
   - link: https://github.com/coala/coala/pull/6037
   - details: >
      ConsoleInteraction.py: Show context for diffs
 - 7:
   - repo: coala
   - link: https://github.com/coala/coala/pull/6063
   - details: >
      ConsoleInteraction.py: Add parents to syntax tree
 - 8:
   - repo: coala
   - link: https://github.com/coala/coala/pull/6064
   - details: >
      Add support for non utf-8 encodings
 - 9:
   - repo: coala
   - link: https://github.com/coala/coala/pull/6067
   - details: >
      Add support for binary diffs

---

### Result Reporter Tool


#### Work Done

1. Writing tests for non utf-8 encodings in both coala and coala-utils. and
adding support for non utf-8 encodings to coala.

2. Improvements to the diff contexts shown by coala by showing lines before
and after the affected code and showing the parents (class/function).

3. Add Support for Showing diffs for binary files. Tested with
ImageCompressionBear.

4. Support for xml diffs.

#### Challenges

Adding support for binary and xml diffs was quite complex which I was not
prepared for beforehand. So I had to come up with designs for these new
types of diffs and how to print them with coala. This required finding the
right libraries for generating diffs for binary and xml files and using them
in coala.

I learned many new concepts along the way. My mentor guided me whenever I got
stuck at a problem and was really influential in the work I have done these
three months.

#### Work to be done

The binary and xml diffs are prototypes and can be improved further.

1. We can show stats for binary diffs. So the diffs will show the additions
and deletions in the diff and only print the binary diff when ShowAppliedPatches
is called.

2. XML Diffs need to be tested with bears which change data so they could be
viewed in all their glory.
