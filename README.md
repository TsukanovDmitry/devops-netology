Домашнее задание к занятию "7.6. Написание собственных провайдеров для Terraform."

1. Давайте потренируемся читать исходный код AWS провайдера, который можно склонировать от сюда: https://github.com/hashicorp/terraform-provider-aws.git. Просто найдите нужные ресурсы в исходном коде и ответы на вопросы станут понятны.

Найдите, где перечислены все доступные resource и data_source, приложите ссылку на эти строки в коде на гитхабе.
https://github.com/Kraktorist/devops-netology/blob/main/07-terraform-06-providers/README.md#:~:text=Answer-,Resources,-Datasources
https://github.com/Kraktorist/devops-netology/blob/main/07-terraform-06-providers/README.md#:~:text=Resources-,Datasources,-name%20%D0%BA%D0%BE%D0%BD%D1%84%D0%BB%D0%B8%D0%BA%D1%82%D1%83%D0%B5%D1%82%20%D1%81

Для создания очереди сообщений SQS используется ресурс aws_sqs_queue у которого есть параметр name.
С каким другим параметром конфликтует name? Приложите строчку кода, в которой это указано.
name конфликтует с name_prefix (https://github.com/hashicorp/terraform-provider-aws/blob/6e6e4bed78f29b0addd5b33fd733b67f85bb4dc3/internal/service/sqs/queue.go#L87)

Какая максимальная длина имени?
80 символов: 
(re = regexp.MustCompile(`^[a-zA-Z0-9_-]{1,80}$`))

Какому регулярному выражению должно подчиняться имя?
Зависит от fifo_queue.

if fifoQueue {
			re = regexp.MustCompile(`^[a-zA-Z0-9_-]{1,75}\.fifo$`)
		} else {
			re = regexp.MustCompile(`^[a-zA-Z0-9_-]{1,80}$`)
		}

		if !re.MatchString(name) {
			return fmt.Errorf("invalid queue name: %s", name)
		}
