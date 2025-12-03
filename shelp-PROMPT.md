You are a expert programmer.

Write a script that uses AI to write shell helpers. You can use
`claude -p "prompt"` which will echo the output of a prompt on the
command line. the command is called shelp (shell help). You're welcome to write
it in bash or python.

If you write:

shelp split input by commas and output in lines with @meta.com added to each item

it would echo to stdout a one liner that does that. it should be a bash
command, but could use python -c if it's the best way to do it.

if you use shelp -o <output> <prompt>  it will use compile mode. instead of outputting
 in one liner style it will output a ready to run executable. In this case,
 prompt the AI to be more expressive in how it writes. The file should be made
 executable. in this case it can use either bash or python (though if the output name
 is .sh or .py it should use the appropriate language).

if -o is passed, still echo the script to stdout, and prompt the user if the
script looks ok before outputting the file. If they say no, open the prompt in
EDITOR allow them to modify it. If they don't change the file, simply abort otherwise use
the modified prompt.

if the user passes --compile, <command>-PROMPT.md read the prompt from that file
and output to <command>.

If the user calls shelp --improve <file>, it should allow the to improve the
prompt of the saved file. In other words, open an EDITOR with the prompt that
created the file and edit it. When the AI is called, you should share with it
the unimproved shell script as well as the user's updated prompt as context.'
You should also keep a changelog as you --improve the file.

If the user calls shelp --improve, you should improve shelp itself.

To implement improvement, you'll need to have the script's prompt. If the script was
built with --compile, use the PROMPT.md file. On the other hand, when using -o, output
the prompt, verbatim inside the script itself. Do not output this metadata when showing
the user the file for confirmation. Remember that the script (including shelp's path)
might be a symlink so resolve the real path to look for the prompt.

--tweak is a variant of improve that also takes an optional argument. it still
opens an editor with the prompt, but the prompt is commented out. There are further
comments explaining to the user that they only need to explain what they would like
to edit about the prompt. In this case, the prompt to claude should instruct claude to
first modify the user's prompt, as per the tweak, then use the modified tweak as the
improvement. The user will make tweaks above teh commented area since that is where
their cursor will be. Be sure to add a few blank lines for them.

--prompt <file> should output the prompt that created the file. If the file is empty
output shelp's prompt. include the changelog.

--help gives a detailed description of how to use shelp with examples.

--install <arg> it should install itself into the <arg> directory and
offer to add <arg> to the PATH in bashrc if it's not already there. if not arg is specified
use ~/bin. if the directory doesn't exist, create it.

`claude` will output text to stderr that you do not need and should not be displayed
to the user only display the output of claude's stderr if it returns a non-zero exit code.

Ensure an expressive prompt is given to the ai. the AI should generally choose
bash and classic unix tools or python, but could use perl one liners if really
helpful.

Now it's time to build shelp. Since shelp doesn't exist you'll have to build it as if the user
had called:

shelp --compile shelp-PROMPT.md.

Then output a brief description of how to use shelp that encourages the user to take next steps,
start with basic examples that output commands to stdout, then build up to outputting files and
finally to compilation. Make the examples engaging and realistic to show the value of shelp.

Finally, encourage the user to use --install to install shelp.

Keep in mind that you are outputting to
a terminal. Keep output to less than 100 characters per line. You can't use markdown, but
emoji will work.
