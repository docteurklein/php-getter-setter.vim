    Vim filetype plugin file for adding getter/setter methods
    Language: PHP 5
    Maintainer: Florian Klein <florian.klein@free.fr>
    Last Change: 2012 Sep
    Revision: $Id$
    Credit:
       - Antoni Jakubiak (antek AT clubbing czest pl)
       - It's modification java_getset.vim by Pete Kazmier

    =======================================================================

    Copyright 2011 by Florian Klein

    Redistribution and use in source and binary forms, with or without
    modification, are permitted provided that the following conditions
    are met:

     1. Redistributions of source code must retain the above copyright
          notice, this list of conditions and the following disclaimer.

     2. Redistributions in binary form must reproduce the above
          copyright notice, this list of conditions and the following
          disclaimer in the documentation and/or other materials provided
          with the distribution.

     3. The name of the author may not be used to endorse or promote
          products derived from this software without specific prior
          written permission.

    THIS SOFTWARE IS PROVIDED BY THE AUTHOR ``AS IS'' AND ANY EXPRESS
    OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
    WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
    ARE DISCLAIMED. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY
    DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
    DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE
    GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
    INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
    WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
    NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
    SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

    =======================================================================

    DESCRIPTION
    This filetype plugin enables a user to automatically add getter/setter
    methods for PHP properties.  The script will insert a getter, setter,
    or both depending on the command/mapping invoked.  Users can select
    properties one at a time, or in bulk (via a visual block or specifying a
    range).  In either case, the selected block may include comments as they
    will be ignored during the parsing.  For example, you could select all
    of these properties with a single visual block.

    class Test
    {
       var $count;

       var $name;

       var $address;
    }


    The getters/setters that are inserted can be configured by the user.
    First, the insertion point can be selected.  It can be one of the
    following: before the current line / block, after the current line /
    block, or at the end of the class (default).  Finally, the text that is
    inserted can be configured by defining your own templates.  This allows
    the user to format for his/her coding style.  For example, the default
    value for s:phpgetset_getterTemplate is:

        /**
         * Get %varname%.
         *
         * @return %varname%
         */
        function %funcname%()
        {
            return %varname%;
        }

    Where the items surrounded by % are parameters that are substituted when
    the script is invoked on a particular property.  For more information on
    configuration, please see the section below on the INTERFACE.

    INTERFACE (commands, mappings, and variables)
    The following section documents the commands, mappings, and variables
    used to customize the behavior of this script.

    Commands:
      :InsertGetterSetter
          Inserts a getter/setter for the property on the current line, or
          the range of properties specified via a visual block or x,y range
          notation.  The user is prompted to determine what type of method
          to insert.

      :InsertGetterOnly
          Inserts a getter for the property on the current line, or the
          range of properties specified via a visual block or x,y range
          notation.  The user is not prompted.

      :InsertSetterOnly
          Inserts a setter for the property on the current line, or the
          range of properties specified via a visual block or x,y range
          notation.  The user is not prompted.

      :InsertBothGetterSetter
          Inserts a getter and setter for the property on the current line,
          or the range of properties specified via a visual block or x,y
          range notation.  The user is not prompted.


    Mappings:
      The following mappings are pre-defined.  You can disable the mappings
      by setting a variable (see the Variables section below).  The default
      key mappings use the <LocalLeader> which is the backslash key by
      default '\'.  This can also be configured via a variable (see below).

      <LocalLeader>p   (or <Plug>PhpgetsetInsertGetterSetter)
          Inserts a getter/setter for the property on the current line, or
          the range of properties specified via a visual block.  User is
          prompted for choice.

      <LocalLeader>g   (or <Plug>PhpgetsetInsertGetterOnly)
          Inserts a getter for the property on the current line, or the
          range of properties specified via a visual block.  User is not
          prompted.

      <LocalLeader>s   (or <Plug>PhpgetsetInsertSetterOnly)
          Inserts a getter for the property on the current line, or the
          range of properties specified via a visual block.  User is not
          prompted.

      <LocalLeader>b   (or <Plug>PhpgetsetInsertBothGetterSetter)
          Inserts both a getter and setter for the property on the current
          line, or the range of properties specified via a visual block.
          User is not prompted.

      If you want to define your own mapping, you can map whatever you want
      to <Plug>PhpgetsetInsertGetterSetter (or any of the other <Plug>s
      defined above).  For example,

          map <buffer> <C-p> <Plug>PhpgetsetInsertGetterSetter

      When you define your own mapping, the default mapping does not get
      set, only the mapping you specify.

    Variables:
      The following variables allow you to customize the behavior of this
      script so that you do not need to make changes directly to the script.
      These variables can be set in your vimrc.

      no_plugin_maps
        Setting this variable will disable all key mappings defined by any
        of your plugins (if the plugin writer adhered to the standard
        convention documented in the scripting section of the VIM manual)
        including this one.

      no_php_maps
        Setting this variable will disable all key mappings defined by any
        php specific plugin including this one.

      maplocalleader
        By default, the key mappings defined by this script use
        <LocalLeader> which is the backslash character by default.  You can
        change this by setting this variable to a different key.  For
        example, if you want to use the comma-key, you can add this line to
        your vimrc:

            let maplocalleader = ','

      b:phpgetset_insertPosition
        This variable determines the location where the getter and/or setter
        will be inserted.  Currently, three positions have been defined:

            0 - insert at the end of the class (default)
            1 - insert before the current line / block
            2 - insert after the current line / block

      g:phpgetset_getterTemplate
      g:phpgetset_setterTemplate
        These variables determine the text that will be inserted for a
        getter, setter, array-based getter, and array-based setter
        respectively.  The templates may contain the following placeholders
        which will be substituted by their appropriate values at insertion
        time:

            %varname%       The name of the property
            %funcname%      The method name ("getXzy" or "setXzy")

        For example, if you wanted to set the default getter template so
        that it would produce the following block of code for a property
        defined as "var $name":

            /**
             * Get name.
             * @return name
             */
           function getName() { return $this->name; }

        This block of code can be produced by adding the following variable
        definition to your vimrc file.

            let g:phpgetset_getterTemplate =
              \ "\n" .
              \ "/**\n" .
              \ " * Get %varname%.\n" .
              \ " * @return %varname%\n" .
              \ " */\n" .
              \ "%public function %funcname%() { return $this->%varname%; }"

        The defaults for these variables are defined in the script.  For
        both the getterTemplate and setterTemplate, there is a corresponding
        array-baded template that is invoked if a property is array-based.
        This allows you to set indexed-based getters/setters if you desire.
        This is the default behavior.

      PhpGetsetProcessFuncname(funcname)
        Function that will be called, if it does exists, to process the setter
        and getter function name.

        For example, the following function will convert upper_camel_case into
        UpperCamelCase.

        function PhpGetsetProcessFuncname(funcname)
            let l:funcname = split(tolower(a:funcname), "_")
            let l:i = 0

            while l:i < len(l:funcname)
                let l:funcname[l:i] = toupper(l:funcname[l:i][0]) . strpart(l:funcname[l:i], 1)
                let l:i += 1
            endwhile

            return join(l:funcname, "")
        endfunction


    INSTALLATION
    1. Copy the script to your ${HOME}/.vim/ftplugins directory and make
       sure its named "php_getset.vim" or "php_something.vim" where
       "something" can be anything you want.

    2. (Optional) Customize the mapping and/or templates.  You can create
       your own filetype plugin (just make sure its loaded before this one)
       and set the variables in there (i.e. ${HOME}/.vim/ftplugin/php.vim)

    =======================================================================

    NOTE:
    This is my first VIM script.  I do not read any documentation.
    I make only some modifications in the original code java_getset.vim.

    Only do this when not done yet for this buffer
