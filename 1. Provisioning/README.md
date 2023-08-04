# Provision servers using Terraform


Saya akan membuat file `main.tf` yang nantinya akan membuat resource vm berbeda dengan menerapkan IaC menggunakan terraform.

```
terraform {
  required_providers {
    idcloudhost = {
      source = "bapung/idcloudhost"
      version = "0.1.3"
    }
  }
}

provider "idcloudhost" {
  auth_token = "mTWvAHka6YmhUncBxKZyCpeL40C7Kyc0"
}

resource "idcloudhost_vm" "appserver" {
  name = "salman-appserver"
  os_name = "ubuntu"
  os_version = "20.04"
  vcpu = 2
  memory = 2048
  disks = 20
  username = "sallfarr"
  initial_password = "Katasand1"
  public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDMY1bm0RTQDwE8zNusvj/VlBP8oZm7TT2Cr43cKGr1q4+5mkyq1EJS3udM7YfwHg+bLfC6yZmyrh1tLMCmiNFpFDTtkcg/rcvtyc1RM0v5XF8Ldy09o+5TvyfZ3ROVl0uhfY35BucHLekjjZjjty+HVXtOgvuhJhSaASs7mMqAfVkkJ//ud57CuoiH+1SckQo/aj7gma7MnLxu0wwcLkqx0GH67XFz1aRsmLpuikhoPXWuTCDtgGnIfYcHNC4W6CPGHd+Ov9jPXnuZiiCC1uG0M/weT0wVK1Snrcs7Gq0Ekles34bXcbrHu8/jhVWx5773kMtkekIg6g6CU/S1fyVTGVSuW+rs5WLkrT/tcH8Aqb9C1kjC7e9S+w0y9ZO60wlWSWH9oAGkqEJxcP9Xt0lagkmIkhuXl12wwLCuWcSu09Jaww86p19LqhBa5rSmF1LWz5l9e7L5qXL/9ClX5SeNxXpmFy4UJdZAqVW6PNoL5LhCw2AwJWNfw6o9Y51N81M= sallf@windows_salman"
  billing_account_id = "1200157626"
}

resource "idcloudhost_vm" "gateway" {
  name = "salman-gateway"
  os_name = "ubuntu"
  os_version = "20.04"
  vcpu = 2
  memory = 2048
  disks = 20
  username = "sallfarr"
  initial_password = "Katasand1"
  public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDMY1bm0RTQDwE8zNusvj/VlBP8oZm7TT2Cr43cKGr1q4+5mkyq1EJS3udM7YfwHg+bLfC6yZmyrh1tLMCmiNFpFDTtkcg/rcvtyc1RM0v5XF8Ldy09o+5TvyfZ3ROVl0uhfY35BucHLekjjZjjty+HVXtOgvuhJhSaASs7mMqAfVkkJ//ud57CuoiH+1SckQo/aj7gma7MnLxu0wwcLkqx0GH67XFz1aRsmLpuikhoPXWuTCDtgGnIfYcHNC4W6CPGHd+Ov9jPXnuZiiCC1uG0M/weT0wVK1Snrcs7Gq0Ekles34bXcbrHu8/jhVWx5773kMtkekIg6g6CU/S1fyVTGVSuW+rs5WLkrT/tcH8Aqb9C1kjC7e9S+w0y9ZO60wlWSWH9oAGkqEJxcP9Xt0lagkmIkhuXl12wwLCuWcSu09Jaww86p19LqhBa5rSmF1LWz5l9e7L5qXL/9ClX5SeNxXpmFy4UJdZAqVW6PNoL5LhCw2AwJWNfw6o9Y51N81M= sallf@windows_salman"
  billing_account_id = "1200157626"
}

resource "idcloudhost_vm" "monitoring" {
  name = "salman-monitoring"
  os_name = "ubuntu"
  os_version = "20.04"
  vcpu = 2
  memory = 1024
  disks = 20
  username = "sallfarr"
  initial_password = "Katasand1"
  public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDMY1bm0RTQDwE8zNusvj/VlBP8oZm7TT2Cr43cKGr1q4+5mkyq1EJS3udM7YfwHg+bLfC6yZmyrh1tLMCmiNFpFDTtkcg/rcvtyc1RM0v5XF8Ldy09o+5TvyfZ3ROVl0uhfY35BucHLekjjZjjty+HVXtOgvuhJhSaASs7mMqAfVkkJ//ud57CuoiH+1SckQo/aj7gma7MnLxu0wwcLkqx0GH67XFz1aRsmLpuikhoPXWuTCDtgGnIfYcHNC4W6CPGHd+Ov9jPXnuZiiCC1uG0M/weT0wVK1Snrcs7Gq0Ekles34bXcbrHu8/jhVWx5773kMtkekIg6g6CU/S1fyVTGVSuW+rs5WLkrT/tcH8Aqb9C1kjC7e9S+w0y9ZO60wlWSWH9oAGkqEJxcP9Xt0lagkmIkhuXl12wwLCuWcSu09Jaww86p19LqhBa5rSmF1LWz5l9e7L5qXL/9ClX5SeNxXpmFy4UJdZAqVW6PNoL5LhCw2AwJWNfw6o9Y51N81M= sallf@windows_salman"
  billing_account_id = "1200157626"
}

resource "idcloudhost_floating_ip" "appserver" {
  name = "appserverIP"
  billing_account_id = 1200157626
  assigned_to = idcloudhost_vm.appserver.id
}

resource "idcloudhost_floating_ip" "ipgateway" {
  name = "gatewayIP"
  billing_account_id = 1200157626
  assigned_to = idcloudhost_vm.gateway.id
}

resource "idcloudhost_floating_ip" "ipmonitoring" {
  name = "monitoringIP"
  billing_account_id = 1200157626
  assigned_to = idcloudhost_vm.monitoring.id
}
```
