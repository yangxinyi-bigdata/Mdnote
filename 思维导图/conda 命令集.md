| **Task**/任务                                         | **Conda package and environment manager command** conda包和环境管理命令 |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| Install a package/安装包                              | conda install $PACKAGE_NAME                                  |
| Update a package/升级一个包                           | conda update --name $ENVIRONMENT_NAME $PACKAGE_NAME          |
| Update package manager/升级conda自身                  | conda update conda                                           |
| Uninstall a package/卸载包                            | conda remove --name $ENVIRONMENT_NAME $PACKAGE_NAME          |
| Create an environment/创建一个新的环境                | conda create --name $ENVIRONMENT_NAME python                 |
| Activate an environment/激活一个新环境                | source activate $ENVIRONMENT_NAME                            |
| Deactivate an environment/停用一个环境                | source deactivate                                            |
| Search available packages/搜索可用的包                | conda search $SEARCH_TERM                                    |
| Install package from specific source/从特殊的源安装包 | conda install --channel $URL $PACKAGE_NAME                   |
| List installed packages/查看已安装包列表              | conda list --name $ENVIRONMENT_NAME                          |
| Create requirements file/创建需求文件                 | conda list --export                                          |
| List all environments/列举出所有已安装环境            | conda info --envs                                            |
| Install other package manager/安装其他包管理软件pip   | conda install pip                                            |
| Install Python/安装python                             | conda install python=x.x                                     |
| Update Python/升级python                              | conda update python *                                        |