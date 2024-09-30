# Basic preparation

## gcc

```shell
sudo apt install gcc
```

- `sudo` : Super User do
- `apt` : **Advanced Package Tool**,simplify the installation, upgrade,and delete.



## File command

```
ls -l
```

- `ls -l` : list all files in the **current directory** in the form of list-items.

  

```shell
cd directory-name
```

- `cd` : go into the directory which you appoint (the son directory)



```shell
cd ..
```

- `cd ..` : go back to the parent directory



```shell
cd /
```

- `cd home` : go back to the root directory



```shell
pwd
```

- `pwd`: show the current path 

![image-20240519185853485](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OSimage-20240519185853485.png)



```shell
rm -rf directory-name
```

- `rm`: remove
- `rf`: 
  - r : recursive
  - f : force
- `rm -rf` force to delete all the files in the appointed directory and the directory



```shell
mkdir dir-name
```

- `mkdir`: *make a new dir*



```shell
touch filename
```

- `touch`: *create a file*

![image-20240519185859482](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OSimage-20240519185859482.png)



```shell
cat helloworld.c
```

- `cat`: check the content of the appointed file

![image-20240519185908429](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OSimage-20240519185908429.png)



```shell
gcc helloworld.c
```

- Gcc : complie the C file , create an 'a.out' file

  ```shell
  gcc helloworld.c -o helloworld
  ```

  Compile the helloworld.c and create an execuable file called 'helloworld'



```shell
./helloworld
```

- `./`: execute the file you pass in
  - `r` : read
  - `w` : write
  - `x` : execute

![image-20240519185915381](https://fakercodes.oss-cn-hangzhou.aliyuncs.com/sl/OSimage-20240519185915381.png)

