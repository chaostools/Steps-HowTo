since i dont keep global credentials( good habit i think), there fore:
0. you can only push changes to the repo that is created/forked in the account you will be using
1. open empty folder .open terminal there. `git clone <url>` .
2. close terminal. <b> open terminal in the cloned repository now .</b>
3. `git init` .
4. set credentials for current session. `git config --global user.name <username>` and `git config --global user.email <email>` .
5. add your desired files now in the project.then go back to terminal.
6. `git add .` then `git commit -m "message"` .
7. <B>` git push - u origin master  `</B>
