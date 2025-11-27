# Chsh non-standard shell

```shell
chsh -s $(which zsh)
Password enter.
chsh: /usr/local/bin/zsh: non-standard shell
```

non-standard shell 오류가 발생하였다.

chsh는 /etc/shells에 있는 것만 쉘로 받아들인다고 한다.

> chsh will accept the full pathname of any executable file on the system. However, it will issue a warning if the shell is not listed in the /etc/shells file.

이 문제를 해결하기 위해서는 먼저 아래의 방법을 시도해본다.

```shell
sudo echo "$(which zsh)" >> /etc/shells
chsh -s $(which zsh)
```

물론 zsh가 설치되어있어야 하고, `brew install zsh` 를 통해 설치하는게 제일 편하다.

위의 명령어가

`/etc/shells: Permission denied` 오류를 출력한다면, 다음을 시도해본다.

```shell
sudo sh -c "echo $(which zsh) >> /etc/shells"
chsh -s $(which zsh)
```

이는 >연산자가 echo가 아닌 shell에 의해 수행되는 것 때문이므로 위와 같이 실행하면 쉘에서 bash 명령어를 root권한으로 실행하는 것이라고 한다.

참고한 사이트: [StackOverFlow](https://stackoverflow.com/questions/31034870/making-zsh-default-shell-in-macosx) , [StackExchange/askubuntu](https://askubuntu.com/questions/230476/how-to-solve-permission-denied-when-using-sudo-with-redirection-in-bash)

