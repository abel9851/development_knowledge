```terraform

resource "local_file" "build_log_file" {

  for_each = {
    for idx, app_name in var.applications : 
      app_name => app_name if app_name != "vue3"
  }

  content         = docker_container.node_builder[each.key].container_logs
  filename        = "build_output_${each.key}.txt"
  file_permission = "0644"
}

resource "local_file" "vue3_build_log_file" {
  for_each = {
    for idx, app_name in var.applications :
       app_name => app_name if app_name == "vue3"
  }

  content         = docker_container.vue3_node_builder[each.key].container_logs
  filename        = "build_output_${each.key}.txt"
  file_permission = "0644"
}

```

위의 코드처럼 terraform에서도 if문을 사용할 수 있다.