# laravel_artisan_autocomplete

#save the below code in your bash rc which is at /home/$USER/.bashrc

#/usr/bin/env bash


_completions_php_artisan()
{
  if [ "${#COMP_WORDS[@]}" != "2" ]; then
    return
  fi

  if [ ! -f command_list.txt ]; then
      eval 'php artisan --raw --no-ansi list | sed "s/[[:space:]].*//g" > command_list.txt'
  else 
    LAST=`stat -c %Y command_list.txt`
    NOW=`date +"%s"`
    DIFF=`expr $NOW - $LAST`
    if [ $DIFF -gt 900 ]; then
      eval 'php artisan --raw --no-ansi list | sed "s/[[:space:]].*//g" > command_list.txt'
    fi

  fi
  COMP_WORDBREAKS=${COMP_WORDBREAKS//:}
  COMPREPLY=($(compgen -W "$(cat command_list.txt)" "${COMP_WORDS[1]}"))
}
alias phpartisan="php artisan"
complete -F _completions_php_artisan phpartisan


#credit goes to dangdungcntt from  https://code.nddcoder.com/v/re0e1x8n
