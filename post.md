__Введение__

Emacs - отличный текстовый редактор для комфортной работы с Python кодом. 

__Основные возможности__

Для начала работы рекомендуется установить удобный менеджер плагинов, с ленивой загрузкой и обновлением, как [NeoBundle](https://github.com/Shougo/neobundle.vim) из vim. Наш выбор - [el-get](https://github.com/dimitri/el-get) - идеальный менеджер с автокомпиляцией, автоматической инициализацией, а также самое важное - рецептами установки. Короткий пример рецепта, для наглядности.

```lisp
(:name flycheck
       :type github
       :pkgname "flycheck/flycheck"
       :checkout "0.25.1"
       :minimum-emacs-version "24.3"
       :description "On-the-fly syntax checking extension"
       :build '(("makeinfo" "-o" "doc/flycheck.info" "doc/flycheck.texi"))
       :info "./doc"
       :depends (dash pkg-info let-alist seq)
       :post-init (progn
                    (add-hook 'python-mode-hook             #'flycheck-mode)
                    (add-hook 'js-mode-hook                 #'flycheck-mode)
                    (add-hook 'web-mode-hook                #'flycheck-mode)
                    (add-hook 'lisp-interaction-mode-hook   #'flycheck-mode)
                    (add-hook 'fish-mode-hook               #'flycheck-mode)
                    (add-hook 'markdown-mode-hook           #'flycheck-mode)
                    (add-hook 'go-mode-hook                 #'flycheck-mode)
                    (setq flycheck-check-syntax-automatically '(mode-enabled save idle-change))
                    (setq flycheck-highlighting-mode 'lines)
                    (setq flycheck-indication-mode 'left-fringe)
                    (setq flycheck-checker-error-threshold 2000)
                    ))
```

Это рецепт установки [flycheck](https://github.com/flycheck/flycheck) - плагина, для синтаксической проверки файлов при редактировании, на лету. В рецепте можно указывать источник кода - репозиторий git/github или просто ссылку на файл или название плагина в ELPA/MELPA, необходимые зависимости, в случае github - ветку или таг.
Команда :build позволяет запускать системные утилиты, если они необходимы для корректной установки плагина.
Также есть замечательная команда - :post-init, в ней можно указывать лисповый код, который будет запускается каждый раз после инициализации плагина. В общем - очень удобный инструмент.

Все настройки расположены в [папке](https://github.com/steelkiwi/emacs.d/tree/master/.emacs.d) ```~/.emacs.d```. Максимально тонкий [init.el](https://github.com/steelkiwi/emacs.d/blob/master/.emacs.d/init.el) в котором подключаются логически разбитые файлы из папки settings.

```lisp
(add-to-list 'load-path (expand-file-name "settings" user-emacs-directory))
(require 'dark-mint-theme)
(require 'scratch_my)
(require 'package_my)
(require 'hooks_my)
(require 'keybindings_my)
...
автоматически генерируемый при customize код
```

Первый файл - тема оформления. Больше тем [тут](https://emacsthemes.com/)

[scratch_my.el](https://github.com/steelkiwi/emacs.d/blob/master/.emacs.d/settings/scratch_my.el) описывает все стандартные настройки и включает необходимые режимы, которые уже присутствуют с нуля в любом свежем Emacs.

В [package_my.el](https://github.com/steelkiwi/emacs.d/blob/master/.emacs.d/settings/package_my.el) список всех устанавливаемых плагинов и несколько функций для корректной установки.

Хуки разных режимов описаны в файле [hooks_my.el](https://github.com/steelkiwi/emacs.d/blob/master/.emacs.d/settings/hooks_my.el).

Смена сочетаний клавиш вынесена в отдельный файл [keybindings_my.el](https://github.com/steelkiwi/emacs.d/blob/master/.emacs.d/settings/hooks_my.el).

Также синхронизируются папка с [сниппетами yasnippet](https://github.com/steelkiwi/emacs.d/tree/master/.emacs.d/snippets) и с [рецептами](https://github.com/steelkiwi/emacs.d/tree/master/.emacs.d/settings/recipes).
Очень часто вносятся изменения в рецепты, добавляются туда настройки плагинов, поэтому они хранятся в отдельной папке.

__Плагины, не зависимые от языка программирования__

Не будем подробно описывать все плагины, к каждому прилагается ссылка, где подробно все расписано.

Начнём пожалуй с [avy](https://github.com/abo-abo/avy), плагина позволяющего переходить на слово, содержащее введённый символ(аналог [vim-easymotion](https://github.com/easymotion/vim-easymotion)).

Для автодополнения используется [company-mode](http://company-mode.github.io/) - универсальное автодополнение для Emacs.
Очень быстро работает, полностью настраивается, хотя документация у него неполная, в исходники заглядывал частенько. Удобство проявляется в структуре данного плагина. Есть бекенд - индивидуальный код для разных режимов и фронтенд - общий код, который вырисовывает результаты автодополнения в самом Emacs. Для разных режимов используются разные бекенды. Например для лиспа используется company-elisp бекенд, для Python - [company-jedi](https://github.com/syohex/emacs-company-jedi)(бекенд для [jedi](https://github.com/davidhalter/jedi) - библиотеки статического анализа кода Python).

[expand-region](https://github.com/magnars/expand-region.el) используется для семантического выделения текста. Более подробно в этом [видео](https://www.youtube.com/watch?v=_RvHz3vJ3kA).

Для работы с git используется три замечательных плагина - [git-gutter](https://github.com/syohex/emacs-git-gutter), [mo-git-blame](https://github.com/voins/mo-git-blame) и [magit](https://magit.vc/). Хочется особо выделить последний плагин - он просто потрясающий, пару касаний и код уже на сервере.

А как же файловый менеджер спросите вы? [neotree](https://www.emacswiki.org/emacs/NeoTree). Интеграция с git присутствует.

Очень полезный плагин - [helm](https://github.com/emacs-helm/helm) - автодополнение к всему. Поиск по открытым буферам([helm-swoop](https://github.com/ShingoFukuyama/helm-swoop)), по недавно открытым файлам, по файлам в текущей директории, поиск по командам, переименование переменных в нескольких буферах и много всего остального. Отличная [статья](http://tuhdo.github.io/helm-intro.html) описывает все возможности данного плагина.

[multiple-cursors](https://github.com/magnars/multiple-cursors.el) - после просмотра данного [видео](https://www.youtube.com/watch?v=jNa3axo40qM) вам обязательно захочется его попробовать:).

Плагин, с помощью которого можно назначить действие на двойное нажатие любых клавиш называется [key-chord](https://www.emacswiki.org/emacs/KeyChord)([видео](http://emacsrocks.com/e07.html))

Аналог [vim-powerline](https://github.com/powerline/powerline), функциональной статусной строки - [powerline](https://github.com/milkypostman/powerline)

[projectile](https://github.com/bbatsov/projectile) - плагин для управления проектами в Emacs. Поиск по проекту, замена по файлам проекта, переход внутри проекта, переключение проекта и множество других полезных функций. Подробности в этой [статье](http://tuhdo.github.io/helm-projectile.html)

[yasnippet](https://www.emacswiki.org/emacs/Yasnippet) - позволяет создавать и использовать сниппеты для разных языков программирования.


__Python mode__

Наконец-то мы переходим к настройке Emacs непосредственно для работы с Python.

Начнём пожалуй с хуков

```
;; Python mode
(defun my-merge-imenu ()
  (interactive)
  (let ((mode-imenu (imenu-default-create-index-function))
        (custom-imenu (imenu--generic-function imenu-generic-expression)))
    (append mode-imenu custom-imenu)))

(defun my-python-hooks()
    (interactive)
    (setq tab-width     4
          python-indent 4
          python-shell-interpreter "ipython"
          python-shell-interpreter-args "-i")
    (if (string-match-p "rita" (or (buffer-file-name) ""))
        (setq indent-tabs-mode t)
      (setq indent-tabs-mode nil)
    )
    (add-to-list
        'imenu-generic-expression
        '("Sections" "^#### \\[ \\(.*\\) \\]$" 1))
    (setq imenu-create-index-function 'my-merge-imenu)
    ;; pythom mode keybindings
    (define-key python-mode-map (kbd "M-.") 'jedi:goto-definition)
    (define-key python-mode-map (kbd "M-,") 'jedi:goto-definition-pop-marker)
    (define-key python-mode-map (kbd "M-/") 'jedi:show-doc)
    (define-key python-mode-map (kbd "M-?") 'helm-jedi-related-names)
    ;; end python mode keybindings

    (eval-after-load "company"
        '(progn
            (unless (member 'company-jedi (car company-backends))
                (setq comp-back (car company-backends))
                (push 'company-jedi comp-back)
                (setq company-backends (list comp-back)))
            )))

(add-hook 'python-mode-hook 'my-python-hooks)
;; End Python mode
```

Для работы плагинов нужно установить пару пакетов через pip

```bash
pip install -U jedi virtualenv flake8
```

Устанавливаем настройки отступов и путь к интерпретатору, выставляем специфические сочетания клавиш, добавляем бекенд company-jedi и настраиваем [imenu](https://www.emacswiki.org/emacs/ImenuMode).

Про автодополнение уже было выше(company-jedi), поиск по файлу и структуре файла(именам классов, переменным, методам и тд.) происходит через imenu(F10), открытие и закрытие файлового менеджера NeoTree происходит при нажатии F7.


Теперь немного о используемых плагинах

[auto-virtualenv](https://github.com/marcwebbie/auto-virtualenv) - Активирует virtualenv для Python файла. Отлично работает, особенно в связке с projectile.

[jedi-core](https://github.com/tkf/emacs-jedi/blob/master/jedi-core.el) - отдаёт company-jedi варианты автодополнения, переходит по определениям, показывает доки к функциям и классам и тд.

[pip-requirements](https://github.com/Wilfred/pip-requirements.el) - как видно из названия - пакет для работы с requirements.

[py-autopep8](https://github.com/paetzke/py-autopep8.el) - позволяет автоматически форматировать код в соответствии с стандартом pep8.

[py-isort](https://github.com/paetzke/py-isort.el) - можно автоматически сортировать все импорты в файле

__Python REPL__

В хуках мы указали какую версию shell мы будем использовать
```
   (setq python-shell-interpreter "ipython"
       python-shell-interpreter-args "-i")

```
При нажатии ```C-c C-c``` - все содержимое текущего буфера передаётся в ipython, где его можно тут же протестировать. 
Если нужно лишь часть кода просмотреть, выделяете её и нажимаете ```C-c C-r```

Также можно использовать встроенный в python-mode деббагер, выполнив команду ```M-x pdb RET```(RET - в емаксе обозначает ентер/return, поначалу это сбивало с толку). Также есть более функциональная версия - [realgud](https://github.com/realgud/realgud)


В качестве примера, есть несколько скриншотов
![image](https://habrastorage.org/files/b30/507/52a/b3050752a5b543d1b29649cb61ac20e0.png)

![image](https://habrastorage.org/files/372/717/a3d/372717a3d78a49dd92bcc5ad7974cd1c.png)

__Другие плагины__

[markdown-mode](https://github.com/defunkt/markdown-mode) - в данном режиме как раз и пишется данная статья. А с помощью [livedown](https://github.com/shime/emacs-livedown) просматривается результат сразу в браузере.

[web-mode](http://web-mode.org/) - позволяет работать с html, css, Django/Jinja2 templates.

[restclient](https://github.com/pashky/restclient.el) - плагин для работы с разнообразными Api. Emacsrocks покажет больше в своём [видео](http://emacsrocks.com/e15.html).

__Заключение__

Жаль что в одной статье невозможно расписать все особенности и тонкости настройки каждого плагина, надеюсь список с коротким описанием позволит читателям узнать что-то новое об этом замечательном редакторе.

__Полезные ссылки__

* [Emacsrocks](http://emacsrocks.com/)

* [Emacs - the Best Python Editor?](https://realpython.com/blog/python/emacs-the-best-python-editor/)
