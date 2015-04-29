Improving Mac Terminal
======================

I spend a lot of my time at Gravitywell using Terminal. Connecting to servers,
managing version control and editing files (I'm a [Vim][0] user). Learning how
to use Terminal can be really powerful for a full stack developer, but opening
up the app and seeing the default interface doesn't exactly shout *"Use me! I'm
awesome!"*.

![Standard bash prompt](standard-prompt.png)

It's pretty drab (even if your using the snazzy *Pro* theme shown above), the
font is spidery, pixelated and hard to read and the information being
shown to me in the prompt is pretty useless as well.  
Clearly this kind of display is potentially useful to some people - but it
is doing nothing for me 99% of the time apart from causing me to make a sort of
"bleugghh" noise at the back of my throat.

After my first few tussles with Terminal I saw that it was a great (some may say
essential) tool, despite the unappealing interface.
I decided to spend the morning making a few teaks, and sat down in front of my
computer with 3 goals in mind.

1. Make the colour more interesting.
2. Use a more stylish typeface that's hopefully more legible.
3. Display a more informative and useful prompt.

This was many years ago and "a few teaks" has turned into a weird and interesting
adventure into the land of command line customisation that I doubt will ever end.

![Adventure time!](/adventure.jpg)

I'm of the opinion that **bespoke** = **better**. A tailor made suit is
always going to be more wearable than something off the shelf at Primark. If I'm
going to be using a piece of software for 30 hours a week or more, it's got to
be right for **me**.  
Developers are part craftsperson and part writer. Like the writer, much of the job we do is
confined to our own heads. However, unless we choose and maintain the right
tools for ourselves we will end up working twice as hard for half the
productivity.

In this article I'd like to explain what I've done to meet my 3 initial goals
and hopefully inspire you to modify your working environment to meet **your**
needs.

The colourscheme
----------------

Ok, I know that picking a colour may seem like the least important part of
customizing software but getting the colour right on something is low hanging
fruit. The right colourscheme can make an immediate and profound difference to your experience.
That being said, getting your colours right in software that your going to be reviewing
or editing code in can be a little tricky. Syntax highlighting, comment colours
and linters all require speific contrasting colours to be able to work
effectively.
It's pretty easy to modify colours in Terminal's preferences window, but I was
keen to leverage a pre-existing colour palette that had been designed for code
and hopefully avoid the aforementioned syntax/comment/linter headache.
I forget where I first saw it, but I've been in love with Ethan Schnoover's
[Solarized][2] theme for a long time. There are Solarized themes available as
downloads for both Terminal and Vim, making it very easy to setup.

![solarized dual display](solarized-dualmode.png)

>"[The theme has been designed] *with both precise [CIELAB][3] lightness
relationships and a refined set of hues based on fixed colour wheel relationships.*"


A lot of thought has been put into this colour palette and it shows. The beige
might not be to everyone's taste but it's great to still be
able to read my screen at 3am because my IDE **doesn't look like this:**

![Neon overload](neonoverload.jpg)

If you'd like to explore more themes, take a look at [terminal.sexy][4]. Don't be put
of by the dubious sounding URL, it's a web app that allows you to create, preview
and share Terminal themes. Themes can be downloaded as an XML
file and imported it into Terminal through the preferences window.  
One "gotcha" with this process is that the file can't be imported unless you
change the file extension to `.terminal`.


The font
--------

Like the colourscheme, I thought this would be a quick win, but I didn't have the
Solarized magic bullet. 
Changing the font used in Terminal is as simple as opening the preferences and
clicking `change` under the font header. Unfortunately the choice of fonts
presented here that are suitable for code editing is rather limited. 
Luckily there's a plethora of fonts available for download on the web and this
quickly opened up my options.

My font of choice was chopping and changing
pretty regularly until I came across the fantastic [Airline][6] plugin for Vim.
Airline looks a bit weird (due to missing characters) unless you use one of
the 14 patched typefaces in the [Powerline fonts][7] repository on github. After
playing around with them I ended up using [Droid sans mono][8].  
Its chunky, reads well and as an Android owner and Google fanboy I feel right at home.



The prompt
----------

My prompt remained largely unchanged for a long time until I came across
[this article][1] written by the excellent Mark Otto (of Twitter bootstrap fame).
In the article he provides a code snippet for your `.bash_profile` to turn your
standard prompt into this:

![Standard bash prompt](mdo-prompt.png)

I loved it and started using it myself straight away. Unfortunately there were a
few issues with it. The prompt would fragment or completely disappear when cycling up or
down through my command history and I encountered horrible line wrapping issues.

The issues were mostly resolved in a [later article][9] and solved entirely by
[this lovely gist][10]. The follow up included code that would *change the colour
of your working git branch depending on its status*.

This seemingly insignificant feature is incredibly useful. I'm constantly
reminded to commit my work, resulting in frequent, small commits rather than
behemoths where I've completely forgotten when, why and what code I've changed.

I coloured the prompt with the help of [this wonderful][11] article by Michael
Plotke and the [colour value table][12] on the Solarized site. Here is a basic
but handy script I made for displaying the colours.
(the value need for Terminal is the XTERM column)

    #!/bin/bash
    #
    # generates an 8 bit color table (256 colors) showing the colors used by
    # Ethan Schnoover's Solarized theme

    printf "\nSWATCH SOLARIZED HEX     16/8 TERMCOL  XTERM/HEX   L*A*B      RGB         HSB        "
    printf "\n------ --------- ------- ---- -------  ----------- ---------- ----------- -----------"
    printf "\n\033[48;5;234m      \033[m base03    #002b36  8/4 brblack  234 #1c1c1c 15 -12 -12   0  43  54 193 100  21"
    printf "\n\033[48;5;235m      \033[m base02    #073642  0/4 black    235 #262626 20 -12 -12   7  54  66 192  90  26"
    printf "\n\033[48;5;240m      \033[m base01    #586e75 10/7 brgreen  240 #585858 45 -07 -07  88 110 117 194  25  46"
    printf "\n\033[48;5;241m      \033[m base00    #657b83 11/7 bryellow 241 #626262 50 -07 -07 101 123 131 195  23  51"
    printf "\n\033[48;5;244m      \033[m base0     #839496 12/6 brblue   244 #808080 60 -06 -03 131 148 150 186  13  59"
    printf "\n\033[48;5;245m      \033[m base1     #93a1a1 14/4 brcyan   245 #8a8a8a 65 -05 -02 147 161 161 180   9  63"
    printf "\n\033[48;5;254m      \033[m base2     #eee8d5  7/7 white    254 #e4e4e4 92 -00  10 238 232 213  44  11  93"
    printf "\n\033[48;5;230m      \033[m base3     #fdf6e3 15/7 brwhite  230 #ffffd7 97  00  10 253 246 227  44  10  99"
    printf "\n\033[48;5;136m      \033[m yellow    #b58900  3/3 yellow   136 #af8700 60  10  65 181 137   0  45 100  71"
    printf "\n\033[48;5;166m      \033[m orange    #cb4b16  9/3 brred    166 #d75f00 50  50  55 203  75  22  18  89  80"
    printf "\n\033[48;5;160m      \033[m red       #dc322f  1/1 red      160 #d70000 50  65  45 220  50  47   1  79  86"
    printf "\n\033[48;5;125m      \033[m magenta   #d33682  5/5 magenta  125 #af005f 50  65 -05 211  54 130 331  74  83"
    printf "\n\033[48;5;61m      \033[m violet    #6c71c4 13/5 brmagenta 61 #5f5faf 50  15 -45 108 113 196 237  45  77"
    printf "\n\033[48;5;33m      \033[m blue      #268bd2  4/4 blue      33 #0087ff 55 -10 -45  38 139 210 205  82  82"
    printf "\n\033[48;5;37m      \033[m cyan      #2aa198  6/6 cyan      37 #00afaf 60 -35 -05  42 161 152 175  74  63"
    printf "\n\033[48;5;64m      \033[m green     #859900  2/2 green     64 #5f8700 60 -20  65 133 153   0  68 100  60"
    printf "\n"

I was feeling pretty happy with myself at this point; the colourscheme was great,
the font was inviting and my bash prompt looked cool and was actually useful to me.

Unfortunately as soon as I fired up an SSH session I was seeing the "bleurgh"
prompt again.

![Default ssh prompt](ssh-bad.png)

Getting around this problem wasn't too hard, I set up a git repository for all
my dot files (`.vimrc`, `.bash_profile`, etc) and created a bash script to
symlink the files to my home directory. Now all I had to do was `git clone` and
run my setup script and I could have the same prompt (and Vim config) in my SSH.

"Lovely" I thought to myself, but now my SSH session looks exactly the same as
my normal Terminal session, how am I going to be able to tell the difference?
What if I accidentally kill a production server because I got confused?

![Custom prompt dissaproves](custom-bad.png)

Solving this problem came in two parts.

Firstly, I needed to be able to differentiate between a "normal" session and an
SSH session. I did some digging on [stackoverflow][13] and found this little gem.

    function is_ssh() {
      if [ -n "$SSH_CLIENT" ] || [ -n "$SSH_TTY" ]; then
        return 0
      # many other tests omitted
      else
        case $(ps -o comm= -p $PPID) in
          sshd|*/sshd) return 0;;
        esac
      fi
      return 1
    }

The function detects SSH environment variables and will return `true` if they are
available (indicating an SSH session is active).

Secondly, I needed to be able to change the output of my prompt based on what type
of session I'm in. This was fairly straight forward to cobble together. I
created a function that returns a prompt based on the result of `is_ssh` and
then assigned it to `export PS1`.


    function get_prompt {
      c="\[\033["
      p="${c}38;5;136\]"

      local_session='\[\033[38;5;240m\]ಠ_ಠ\[\e[m\] \[\033[38;5;37m\]\W/\[\e[m\]\[$(git_color)\]$(git_branch)\[\033[m\] '
      ssh_session='\[\033[38;5;240m\]\u\[\033[38;5;37m\]@\h\[\em\]\[\033[38;5;125m\] (\@)\[\em\]\[\e[m\] \[\033[38;5;37m\]\W/\[\e[m\]\[$(git_color)\]$(git_branch)\[\033[m\] '

      n="${c}m]"
      if is_ssh; then
        echo -e "${ssh_session}"
      else
        echo -e "${local_session}"
      fi
    }

    export PS1=$(get_prompt)

The prompt returned by the function when I'm in an SSH session shows the standard
`user@host` ( `\u@\h` ) as well as the current time ( `\@` ).  
Now I can quickly see if I'm on a remote machine and ensure that no horrible
command line related accidents happen!

![A better SSH prompt](ssh-better.png)

One thing still bugged me about this prompt though. The host name `ip-172-31-9-157`
is pretty useless to me. I, like many other developers and sysadmins out there, use a
host config file to create aliases for the machine I work on. A typical entry in
this file looks like this:

    Host myserver
      Hostname 123.45.6.78
      User ec2-user
      IdentityFile ~/path/to/some/private/key

With this config file I can connect to a machine by typing `ssh myserver` instead of
`ssh ec2-user@123.45.6.78`. If I could get the prompt to display the alias
I had set then I would definitely know which server I was one. However this is
much easier said than done and coming up with a clean method of doing it proved
to be a bit tricky.

One option would be to [change the hostname][15] within the machine itself, however
this was something I wanted to avoid for a number of reasons.

- Setting up a hostname on each machine I work on would be massively time consuming and pretty
boring.
- If I ever changed my local alias I'd have to update the machines hostname.
- If a team mate logged onto a server it could cause problems for them.

What I needed was an approach that involved as little setup as possible, that left
the machines hostname untouched and could update dynamically based on my local
config.

The solution I came up with feels like a bit of a hack but has been working
without issue.
In essence, I use the `[command]` parameter of `ssh` to set a Bash variable
and then open a new bash session before the SSH connection closes.
I created a function in my `.bash_profile` called **tunnel** that wraps the ssh command and adds
arguments, passing my hostname alias through the `[command]` parameter.

    function tunnel {
      local HOSTALIAS="'export MYHOSTALIAS=$1; /bin/bash -il'"
      ssh "$@" -t \'"$HOSTALIAS"\'
    }

In the ssh session bash prompt I replaced the ["escape sequence"][14] for the current
machines hostname (`\h`) with a function call. This function will return the
the value of the variable `MYHOSTALIAS` or the current machines hostname if it
isn't available.

    function host_alias {
      if [ -z "$MYHOSTALIAS" ]; then
        echo -e $(hostname -s)
      else
        echo $MYHOSTALIAS
      fi
    }

The resulting prompt string now looks like this:

    ssh_session='\[\033[38;5;240m\]\u\[\033[38;5;37m\]@$(host_alias)\[\em\]\[\033[38;5;125m\] (\@)\[\em\]\[\e[m\] \[\033[38;5;37m\]\W/\[\e[m\]\[$(git_color)\]$(git_branch)\[\033[m\] '

And the rendered bash prompt now looks like this:

![The bestest ssh prompt](ssh-bestest.png)

Now I have no problem seeing which server I'm on and I'm not going to be annoying
any co-workers!

My Terminal *feels* right for me, it's really only cosmetic differences that
i've discussed but it's had a huge difference on my user experience.  
By making the software my own I've ended up with something that
I actually enjoy using and my work is that much better for it.

[0]: https://jaxbot.me/articles/why-i-use-vim
[1]: http://markdotto.com/2012/10/18/terminal-hotness/
[2]: http://ethanschoonover.com/solarized
[3]: http://en.wikipedia.org/wiki/Lab_colour_space
[4]: http://terminal.sexy/
[5]: http://en.wikipedia.org/wiki/Analysis_paralysis
[6]: https://github.com/bling/vim-airline
[7]: https://github.com/powerline/fonts
[8]: http://www.google.com/fonts/specimen/Droid+Sans+Mono
[9]: http://markdotto.com/2013/01/13/improved-terminal-hotness/
[10]: https://gist.github.com/clozed2u/4971506#file-gistfile1-sh
[11]: http://bitmote.com/index.php?post/2012/11/19/Using-ANSI-Color-Codes-to-Colorize-Your-Bash-Prompt-on-Linux
[12]: http://ethanschoonover.com/solarized#the-values
[13]: http://stackoverflow.com/
[14]: http://www.tldp.org/HOWTO/Bash-Prompt-HOWTO/bash-prompt-escape-sequences.html
[15]: http://www.cyberciti.biz/faq/linux-change-hostname/
