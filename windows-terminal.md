# Windows terminal

Para configurarla, este post detalla como hacerlo:

https://www.developerro.com/2020/11/18/custom-windows-terminal/

Si tienes instalado git for windows, puedes editar el fichero .bashrc que hay en la raiz de tu propio usuario. Yo lo tengo así:

````
cd C:/Users/jcl/Documents/Source/Repos;
alias gs="git status";
alias gps="git push";  
alias gc="git commit";
alias gl="git log --graph --abbrev-commit --oneline";
````

La primera linea, para que vaya al directorio de inicio que quiero. Las otras me añaden alias útiles
