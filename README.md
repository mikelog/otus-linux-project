# AWX. Создание workflow по инициализации кластера одной кнопкой.    
1. Исходные данные:    
    1.1. Виртуализация PROXMOX.    
2. Что необходимо для реализации:    
    2.1. Сервер AWX.    
    2.2. Репозиторий github  или gitLab.    
    2.3. Browser и любой редактор или IDE.    
3. Этапы выполнения:    
    3.1. Создать из шаблона или с нуля машины для:    
        - AWX    
        - GitLab(можно использовать внешние источники)  
        
    3.2. Установить необходимый софт, awx  можно запускать в  docker, более подробные инструкции [здесь](https://github.com/ansible/awx/blob/devel/INSTALL.md).  
    
    3.3. Установить GitLab CE, существует поверье, что  можно поставить и EE версию, без оплаты лицензии функционала EE не будет, но  лучше сразу ставить CE, если вы не планируете использовать в будущем все что дает Enterprize Edition. Инструкция [здесь](https://about.gitlab.com/install/#centos-7). И не забываем EE заменять на CE.
    
    3.4. Так же необходим доступ к API Proxmox и к шелл командам, как qm set. Если Proxmox у вас не мажорного релиза, то придется патчить  модули proxmox.py и proxmox_kvm.py. У меня в репозиотрии они уже изменены. Если этого не сделать, то на минорных релизах Proxmox при подключение к API из Ansible будете получать сообщение об ошибке авторизации.  
    
    3.5. Написание плейбука по настройке кластера Kubernetes(для демонстрации)  с одной мастер нодой и  двумя воркерами. 
    
    3.6. Написание плейбуков по созданию VM в таком количестве, которое требуется для роли/плейбука по установке/настройке кластера.
    
    3.7. Настраиваем AWX в соответсвии с необходимым нам разделением на команды, регистрируем пользователей. Подключаем репозитории с  ролями/плейбуками.  
    
    3.8. Создаем workflow из двух ранее созданных шаблонов по запуску плейбуков:    
        - создать VM.    
        - создать кластер. 
        
      В workflow первым указываем плейбук который генерит VM, вторым, на event успешности выполнения первого плейбука, ставим плейбук по разворачиванию кластера на подготовленное окружение. 
      Разработчикам даем  доступ на выполение этого workflow.  
      
      AWX - достаточно удобная и мощная система. Что мне очень понравилось это то, что один раз настраиваешь разные Credentials и потом просто их используешь, не надо ломать голову, а где хранить кей-пасс от vault и так далее, потому что все credentials уже будут зашифрованы. Гибкость настроек того, какой и за каким плейбуком, стартовать плейбук, ограничение доступа, inventory script, возможность из плейбука формировать inventory и назначать его в настройках шаблона, scheduler. 
      
      Ну и самое банальное, если у админа куча  плейбуков по настройке серверов или для каких-то других задач, то можно все это сложить в одном месте и запускать по нажатию кнопки, я не противник Console-mode = On, но почему не пользоваться удобствами?  
