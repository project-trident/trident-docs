**Note:** Aside from section titles and around code-blocks, this file
        uses the standard reStructuredText markup used in trident
	project documentation. Markdown does some light formatting, so
	be sure to view the "Raw" file to use it as an .rst reference.

# Making Documentation Changes

- [Make a Simple Documentation Change](#sidoch)
- [Advanced Documentation Changes](#addochs)
	- [Required Applications](#reaps)
	- [Prepare a local copy of the GitHub repository](#prlocogire)
	- [Basic Git Commands](#bagicos)
	- [Sphinx Structure](#spst)
	- [Documentation Workflow](#dowo)
	- [Update Translation Files](#uptrfis)
- [Documentation Conventions](#docos)
	- [Tables](#tas)
	- [Images](#ims)
	- [Admonition Boxes](#adbos)

|trident| is always looking for documentation contributions from its
users. The project currently has a large amount of documentation, and
the community is instrumental in keeping the information up to date and
providing tips and instructions to solve specific problems. However, the
sheer amount of documentation available coupled with the specific
documentation tools can make contributing appear daunting. Actually, the
reverse is true: **contributing to the documentation is easy!**

## Make a Simple Documentation Change <a name="sidoch"></a>

.. tip:: These instructions are for simple modifications of the
   |trident| handbook, but they also apply to the |lumina| and |sysadm|
   documentation! |lumina| documentation is stored in the
   `lumina-docs <https://github.com/trident/lumina-docs>`_ repository
   and |sysadm| guides are in
   `sysadm-docs <https://github.com/trident/sysadm-docs>`_.

Making a documentation change can be as simple as using a web browser.
A GitHub account is required to submit patches to |trident|, so open a
web browser and log in to GitHub. Making an account is also a simple
process, but be sure to use an often checked email address, as all
communication regarding patches and pull requests are sent to this
address.

Navigate to the `trident-docs <https://github.com/trident/trident-docs>`_
GitHub repository. Click on the :file:`trident-handbook` directory to
view all the documentation files. Open the :file:`.rst` file
corresponding to the chapter needing an update. The chapter names are
reflected in the title of the :file:`.rst` files.
:numref:`Figure %s <docchange1>` shows the trident-docs repository and
the contents of the :file:`trident-handbook` directory.

.. _docchange1:
.. figure:: images/docchange1.png
   :scale: 100%

   Contents of :file:`trident-handbook`

Open the desired chapter file by clicking its entry in the list.

.. tip:: :file:`trident.rst` is the primary index file and should be
   ignored.

Begin editing the file by clicking the :guilabel:`Pencil` icon in the
upper right corner above the file's text. The file moves to *edit* mode,
where it is now possible to make any necessary changes, as
:numref:`Figure %s <docchange2>` shows.

.. _docchange2:
.. figure:: images/docchange2.png
   :scale: 100%

   Editing :file:`install.rst` with GitHub

If making a simple change, it is recommended to avoid adjusting the
specific formatting elements and instead work within or around them.

Once satisfied, scroll to the bottom of the page and write a detailed
commit summary of the new changes. Click :guilabel:`Propose file change`
(green button), then :guilabel:`Create pull request` to submit the
changes to the project. GitHub then does an automated merge check. Click
:guilabel:`Create pull request` again to submit the change to the
repository. The final step is for a developer or project committer to
review the changes, merging or asking for more changes as necessary.

.. tip:: Housekeeping: Once the pull request is merged, delete the now
   obsolete patch branch.

## Advanced Documentation Changes <a name="addochs"></a>

.. note:: These instructions are designed for users running |trident|.
   Actual commands and workflow may change when using a different
   operating system.

Advanced changes to the |trident| documentation require an understanding
of the underlying tools and markup language. This section covers
downloading and installing the required tools and source files to build
a local copy of the documentation. It also discusses the reStructured
Text markup language and some of the specific conventions |trident| uses
in its documentation. It is recommended the contributor be familiar with
using trident and/or FreeBSD before following these instructions.

### Required Applications <a name="reaps"></a>

There are a few packages to install before making a local copy of the
documentation. Sphinx and its relevant extensions are the most
important.

Open |appcafe| or a command-line and download the :command:`py27-sphinx`
package:

:samp:`[user@example] sudo pkg install py27-sphinx`

Press :kbd:`y` if prompted to continue installing the package. Next,
install the :command:`py27-sphinxcontrib-httpdomain` package:

:samp:`[user@example] sudo pkg install py27-sphinxcontrib-httpdomain`

Be sure :command:`git` is installed. |trident| installs this by default.
A GitHub account is also required to follow these instructions. Open a
web browser pointed to https://github.com/ to create an account.

The last critical item to have on hand is a configurable text editor.
The
`Lumina Text Editor <https://lumina-desktop.org/handbook/luminautl.html#text-editor>`_
is a simple plaintext editor built in to |trident| which works very well
when editing :file:`.rst` files, but other editors like :command:`kate`
and :command:`scite` also function well.

### Preparing a local copy of the GitHub repository <a name="prlocogire"></a>

Once ready with Sphinx and extensions installed, navigate to the
`trident-docs <https://github.com/trident/trident-docs>`_ repository and
*fork* it by clicking the :guilabel:`Fork` button in the upper-right
corner of the repository. This creates a copy of the repository on
the user's personal GitHub account, allowing the user to create patches
and submit pull requests to the **upstream** (or *base*) repository.

Now there are two repositories to track on GitHub, the primary
:file:`trident-docs` and the user's forked version on their personal
account.

In the command line, use :command:`git clone` to clone the **forked**
repository:

:samp:`[user@example] git clone https://github.com/[github_user]/trident-docs.git`

:command:`cd` into the newly downloaded :file:`/trident-docs/` directory
to continue configuring this cloned repo.

Set up the local clone of the **forked** repository to point to the
**upstream** repository:

.. tip:: GitHub also documents this procedure in two steps:
   `Configuring a remote for a fork <https://help.github.com/articles/configuring-a-remote-for-a-fork/>`_
   and
   `Syncing a fork <https://help.github.com/articles/syncing-a-fork/>`_.

```
.. code-block:: none

   [user@example] ~/trident-docs% git remote -v
   origin  https://github.com/[github_user]/trident-docs.git (fetch)
   origin  https://github.com/[github_user]/trident-docs.git (push)
   [user@example] ~/trident-docs% git remote add upstream https://github.com/trident/trident-docs.git
   [user@example] ~/trident-docs% git remote -v
   origin  https://github.com/[github_user]/trident-docs.git (fetch)
   origin  https://github.com/[github_user]/trident-docs.git (push)
   upstream    https://github.com/trident/trident-docs.git (fetch)
   upstream    https://github.com/trident/trident-docs.git (push)
```

This configuration allows changes made in a fork to be synced to the
original repository and changes in the original synced to the fork:

```
.. code-block:: none

   [user@example] ~/trident-docs% git fetch upstream
   [user@example] ~/trident-docs% git merge upstream/master
   [user@example] ~/trident-docs% git push origin master
```

One last element to configure is to set the account identity:

```
.. code-block:: none

   [user@example] ~/trident-docs% git config --global user.email "[you@example.com]"
   [user@example] ~/trident-docs% git config --global user.name "[Your Name]"
```

This sets the *global* user name and email for :command:`git`. Remove
**--global** from the command to set the value only for the
:file:`trident-docs` project directory.

.. tip:: It can be very useful to have two local copies of the
   documentation. One to pull in changes from "upstream" and one to
   make local changes and build test. This helps prevent merge conflicts
   where the local changes accidentally override brand new patches added
   to the upstream repository.

### Basic Git commands <a name="bagicos"></a>

Once the local :file:`trident-docs` copy is configured, there are a few
general :command:`git` commands to remember when working in the
directory:

* :command:`git pull`: Update the local copy from the forked GitHub
  repository.

* :command:`git add [path/to_file]`: Designate a local file to stage for
  commit to the forked repository.

* :command:`git status`: Display a message showing what is staged for
  commit and other relevant information.

* :command:`git commit`: Create a patch for the forked repository.
  Writing a commit message describing the changes is always recommended.

* :command:`git push`: Send the patch created using
  :command:`git commit` upstream to the user's forked repository.

GitHub provides a variety of
`introductory guides <https://guides.github.com/>`_ for users new to
its unique workflow. It is recommended to use these guides if confused
about any stage of the commit/push/pull request process.

### Sphinx Structure <a name="spst"></a>

|trident| uses the
`Sphinx Documentation Generator <http://www.sphinx-doc.org/en/stable/#>`_
for all its documentation. Sphinx uses
`reStructuredText <http://docutils.sourceforge.net/rst.html>`_ source
files to generate a variety of output formats, including HTML, LaTeX,
and ePub. The Sphinx builder also tests the markup as it builds,
notifying the user of errors and approximate locations in the file.
Sphinx also supports numerous extensions and customizable elements,
including the output theme, configuration file, and other open-source
options.

.. note:: The
   `Sphinx Project documentation <http://www.sphinx-doc.org/en/stable/contents.html>`_
   is very robust and is recommended to browse or reference it when
   making advanced changes to the |trident| documentation. The Sphinx
   `reStructuredText Primer <http://www.sphinx-doc.org/en/stable/rest.html>`_
   is also highly recommended.

The |trident| implementation of Sphinx uses a few management files and
directories to handle and build the :file:`.rst` source files:

* :file:`trident.rst`: The master index file for the handbook. This file
  governs how the Table of Contents is constructed and which
  :file:`.rst` files to include when starting a build.

* :file:`conf.py`: The Python configuration file for the Sphinx project.
  After the initial setup and some customization, this file is generally
  static.

* :file:`images` directory: All images used in the documentation are
  stored in this directory. Every image is :file:`.png` format, and
  adding or removing an image to this directory requires updating the
  :file:`trident-docs/port-files/pkg-plist` file.

* :file:`themes` directory: Houses the currently used *trident_style*
  theme. This theme includes :file:`html`, :file:`.js`, and :file:`.css`
  files, plus a custom font. These files govern how the documents look
  after an **html** build.

* :file:`Makefile`: This file houses all the specific Sphinx
  :command:`make` commands.

### Documentation Workflow <a name="dowo"></a>

Once all the repository forking and configuration is done, the actual
workflow to make and submit documentation changes is straightforward:

* :command:`cd` into the local copy:
  :samp:`[user@example] ~% cd /trident-docs/trident-handbook`

* Use :command:`git pull upstream master` to download any changes from
  the upstream repository. Then type
  :command:`git merge upstream/master` to add those changes to the local
  copy of the forked repository. Finally, use
  :command:`git push origin master` sync these changes from the upstream
  repository the online forked repository.

* Make any changes to the :file:`.rst` files using a plaintext editor.

* Build test the changes with :command:`make html`. The builder output
  posts messages if any errors are detected. The built :file:`html`
  files are viewable by opening them in a web browser:
  :samp:`[user@example] ~/trident-docs/trident-handbook% firefox _build/html/trident.html &`

* Clean the :file:`_build` directory with :command:`make clean`. The
  contents of this directory are not needed in the online repositories,
  only when conducting build tests.

* Stage a changed file for commit with
  :command:`git add [path/to_file]`.

* Continue changing, testing, and staging files as desired, then make
  the patch to push to the online forked repository with
  :command:`git commit`. Be sure to add a descriptive commit message
  about the changes.

* :command:`git push origin master` to send the patch to the online
  forked repository.

* Open a browser and navigate to the forked :file:`trident-docs`
  repository. Look for the message saying "This branch is 1 commit ahead
  of trident:master" and click the :guilabel:`Pull request` button on the
  same line. GitHub checks if the branches can be automatically merged,
  displaying a *green checkmark* if everything looks good. Click
  :guilabel:`Create pull request`, add any more details to the commit
  message, if necessary, then click :guilabel:`Create pull request`
  again.

* Finished! The patch is submitted for a developer or project maintainer
  to review and merge.

### Update translation files <a name="uptrfis"></a>

Once the initial patch is submitted, it is recommended to submit another
patch to update the translation files:

* :command:`cd` into the local copy and run :command:`make i18n`.

* Once the command is finished, type :command:`make clean`. This removes
  a number of unnecessary files. Type :command:`git status` to view the
  newly updated translation files.

* Commit all these files to a patch with :command:`git commit -a`. Use
  the same commit message as the last non-translation change, but add
  **:TRANSLATIONS** to the title. This allows the project contributors
  to more easily track when and which translation files are updated.

* Follow the same :command:`git push` and GitHub website instructions
  listed above to submit the patch to the upstream repository.

## Documentation Conventions <a name="docos"></a>

This section is intended to provide references for the specific
conventions of |trident| documentation. :numref:`Table %s <specdocconv>`
provides specific conventions of the |trident| project:

.. tip:: It is also recommended to open one of the handbook :file:`.rst`
   files for reference.
   
```
.. _specdocconv:
.. table:: |trident| documentation conventions

   +-----------------+------------------------------+------------------------------------+
   | Convention      | Description                  | Exceptions/Examples                |
   +=================+==============================+====================================+
   | 70 character    | Start a new line every 70    | Certain elements like a long link  |
   | lines           | characters.                  | or code block.                     |
   +-----------------+------------------------------+------------------------------------+
   | Archive old     | Replaced images are moved to | N/A                                |
   | images          | the :file:`archived_images`  |                                    |
   |                 | directory.                   |                                    |
   +-----------------+------------------------------+------------------------------------+
   | PNG images      | All images are in the        | N/A                                |
   |                 | :file:`.png` format.         |                                    |
   +-----------------+------------------------------+------------------------------------+
   | Update plist    | Update                       | N/A                                |
   |                 | :file:`port-files/pkg-plist` |                                    |
   |                 | whenever the                 |                                    |
   |                 | :file:`images/` directory is |                                    |
   |                 | changed.                     |                                    |
   +-----------------+------------------------------+------------------------------------+
   | Whitespace      | Remove any unnecessary       | Empty lines and at the end of      |
   |                 | whitespace.                  | lines.                             |
   +-----------------+------------------------------+------------------------------------+
   | guilabel        | Graphical elements: buttons, | Click                              |
   |                 | icons, fields, columns, and  | :guilabel:`Ok`                     |
   |                 | boxes.                       |                                    |
   +-----------------+------------------------------+------------------------------------+
   | menuselection   | Menu selections and paths    | Select                             |
   |                 |                              | :menuselection:`Foo --> Bar`       |
   +-----------------+------------------------------+------------------------------------+
   | command         | Commands                     | Use the :command:`lcp` command     |
   +-----------------+------------------------------+------------------------------------+
   | file            | File, volume, and dataset    | Locate the                         |
   |                 | names                        | :file:`/etc/rc.conf` file.         |
   +-----------------+------------------------------+------------------------------------+
   | kbd             | Keyboard keys                | Press the :kbd:`Enter` key.        |
   +-----------------+------------------------------+------------------------------------+
   | **Bold**        | Important points             | **This is important.**             |
   +-----------------+------------------------------+------------------------------------+
   | *Italic*        | Device names or values       | Enter *127.0.0.1*                  |
   |                 | entered into fields          | in the address field.              |
   +-----------------+------------------------------+------------------------------------+
   | samp            | Command line representations | :samp:`[user@samp] ~% ls /etc`     |
   +-----------------+------------------------------+------------------------------------+
   | code-block      | Multi-line code examples.    | ``.. code-block:: json``           |
   +-----------------+------------------------------+------------------------------------+
   | External links  | Hyperlink to website outside | Check                              |
   |                 | the built documentation.     | `Google <https://www.google.com>`_ |
   +-----------------+------------------------------+------------------------------------+
   | Internal Links  | Hyperlink to an internal     | See :ref:`Documentation`           |
   |                 | section of the documentation |                                    |
   +-----------------+------------------------------+------------------------------------+
```

:numref:`Table %s <rstmarkup>` provides a basic reference for some of
the often used elements of the reStructuredText markup language. See the
`Sphinx reStructuredText Primer <http://www.sphinx-doc.org/en/stable/rest.html>`_
for a more complete reference.

```
.. _rstmarkup:
.. table:: reStructuredText Markup Reference

   +-------------+-----------------------------------------------------+
   | Markup Type | Description                                         |
   +=============+=====================================================+
   | Paragraph   | Test separated by one or more blank lines. All      |
   |             | lines of the same paragraph must be left-aligned to |
   |             | the same level of indentation.                      |
   +-------------+-----------------------------------------------------+
   | Inline      | One asterisk around text is *italics*.              |
   +-------------+-----------------------------------------------------+
   | Inline      | Two asterisks around text is **bold**.              |
   +-------------+-----------------------------------------------------+
   | Inline      | Two backquotes around text is a code sample.        |
   +-------------+-----------------------------------------------------+
   | Inline role | Interpreted text roles: syntax is                   |
   |             | *:rolename:`content`*.                              |
   +-------------+-----------------------------------------------------+
   | Bullet List | Use :kbd:`*` for bullet list entries.               |
   +-------------+-----------------------------------------------------+
   | Number List | Use :kbd:`1.`, :kbd:`2.`, ... for a numbered list.  |
   +-------------+-----------------------------------------------------+
   | Nested List | Nested lists are separated from the parent items by |
   |             | blank lines.                                        |
   +-------------+-----------------------------------------------------+
   | Quoted par. | Add indentation.                                    |
   +-------------+-----------------------------------------------------+
   | Line blocks | Use :kbd:`|` on the left side to preserve a quote's |
   |             | line breaks.                                        |
   +-------------+-----------------------------------------------------+
   | Comment     | Start line with *..* and a whitespace. End the      |
   |             | comment by adding an empty line, then starting the  |
   |             | next line at the same level of indentation.         |
   +-------------+-----------------------------------------------------+
   | Escape      | Use a :kbd:`\\` (backslash).                        |
   +-------------+-----------------------------------------------------+
```

### Tables <a name="tas"></a>

Tables are all built as grid tables. Here is an example table to copy,
paste, and rework when necessary:

```
.. code-block:: none

   +------------------------+------------+----------+----------+
   | Header row, column 1   | Header 2   | Header 3 | Header 4 |
   | (header rows optional) |            |          |          |
   +========================+============+==========+==========+
   | body row 1, column 1   | column 2   | column 3 | column 4 |
   +------------------------+------------+----------+----------+
   | body row 2             | ...        | ...      |          |
   +------------------------+------------+----------+----------+
```

### Images <a name="ims"></a>

Images are referenced using the numref role and internal marker:

```
.. code-block:: none

   :numref:`Image %s <example1>` is the example syntax to introduce a
   new image.

   .. _example1:
   .. figure:: images/example1.png
      :option: [value]

      Caption for Example1.png
```

### Admonition Boxes <a name="adbos"></a>

These are the specific admonition boxes used by |trident| documentation:

.. tip:: A trivial shortcut or efficiency.

.. note:: Useful information or reminder for accurate command use.

.. warning:: Caution about potential problems introduced by the
   procedure or its misuse.

.. danger:: Extreme caution. Data loss or system damage can occur.
