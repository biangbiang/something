访问控制
=======

客体不受它们所依存的系统的限制，可以包括记录、数据块、存储页、存储段、文件、目录、目录树、库表、邮箱、消息、程序等，还可以包括比特位、字节、字、字段、变量、处理器、通信信道、时钟、网络结点等。

### 自主访问控制

管理的方式不同就形成不同的访问控制方式。一种方式是由客体的属主对自己的客体进行管理，由属主自己决定是否将自己客体的访问权或部分访问权授予其他主体，这种控制方式是自主的，我们把它称为自主访问控制（Discretionary Access Control——DAC）。在自主访问控制下，一个用户可以自主选择哪些用户可以共享他的文件。Linux系统中有两种自主访问控制策略，一种是9位权限码（User-Group-Other），另一种是访问控制列表ACL（Access Control List）。

### 强制访问控制

强制访问控制（Mandatory Access Control——MAC），用于将系统中的信息分密级和类进行管理，以保证每个用户只能访问到那些被标明可以由他访问的信息的一种访问约束机制。通俗的来说，在强制访问控制下，用户（或其他主体）与文件（或其他客体）都被标记了固定的安全属性（如安全级、访问权限等），在每次访问发生时，系统检测安全属性以便确定一个用户是否有权访问该文件。其中多级安全（MultiLevel Secure, MLS）就是一种强制访问控制策略。
