---
  
  title: Using numbers in terraform templates
  date: 2018-02-15
---
  
[Terraform templates](https://www.terraform.io/docs/providers/template/index.html) provide a way to encapsulate common options by using a JSON file.

Passing text is easy

```
# template file
data "template_file" "task_definition" {
  template = "${file("${path.module}/task-definition.json")}"

  vars {
    image_url        = "ghost:latest"
    container_name   = "hello"
    log_group_region = "${var.deploy_region}"
    log_group_name   = "${aws_cloudwatch_log_group.app.name}"
  }
}
```

```
#task-definition.json
[
    {
      "cpu": 256,
      "essential": true,
      "image": "${image_url}",
      "memory": 512,
      "name": "${container_name}",
      "portMappings": [
        {
          "containerPort": 2568,
          "hostPort": 80
        }
      ],
      "logConfiguration": {
          "logDriver": "awslogs",
          "options": {
              "awslogs-group": "${log_group_name}",
              "awslogs-region": "${log_group_region}"
          }
      }
    }
  ]
```

This is good but it would be better if I can use the same file for multiple tasks and get rid of all the hard coded values.

My first try to use a variable for the cpu value as
```
"cpu":"cpu"
```

The solution will definitely break any linters you have running as the resulting JSON file can't be parsed outside of terraform, but it can be done.

Just ignore those red lines ☹️

```
# template file
data "template_file" "task_definition" {
  template = "${file("${path.module}/task-definition.json")}"

  vars {
    image_url        = "ghost:latest"
    container_name   = "hello"
    log_group_region = "${var.deploy_region}"
    log_group_name   = "${aws_cloudwatch_log_group.app.name}"
    container_port   = 2368
    host_port        = 80
    cpu              = 256
    memory           = 512
  }
}
```

```
# task-definition.json
[
    {
      "cpu": ${cpu},
      "essential": true,
      "image": "${image_url}",
      "memory": ${memory},
      "name": "${container_name}",
      "portMappings": [
        {
          "containerPort": ${container_port},
          "hostPort": ${host_port}
        }
      ],
      "logConfiguration": {
          "logDriver": "awslogs",
          "options": {
              "awslogs-group": "${log_group_name}",
              "awslogs-region": "${log_group_region}"
          }
      }
    }
  ]
```




