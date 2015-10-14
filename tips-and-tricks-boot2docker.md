# Tips and tricks for boot2docker

## SSH, users and passwords

* You can SSH into your boot2docker VM by typing `boot2docker ssh` without providing a password. You will then be authenticated as user 'docker'.
* If you use the standard SSH command on your machine, you will have to provide a password. The password for user 'docker' is 'tcuser'.
* The password for the root user is widely unknown, but you can easily change from the 'docker' user to the root user by typing 'sudo su' ;-)

## Quickly starting your development environment on Windows

If you want a one-liner to configure everything on your Powershell session for working with docker, copy the following script into a file
named 'docker.psm1' and place this file in your Powershell profile path (in this case: %USERPROFILE%\\Documents\\WindowsPowerShell\\Modules\\docker)

~~~ powershell
function exists($tool) {
  Write-Host "Searching for $tool ..."
  $cmd = gcm $tool -ErrorAction SilentlyContinue
  if ($cmd -ne $Null) {
    Write-Host "Found $($cmd.Path)" -ForegroundColor Green
    return $True
  } else {
    Write-Host "$tool could not be found" -ForegroundColor Red
    return $False
  }
}

function Start-DockerEnvironment {
  if ((exists boot2docker) -and (exists docker) -and (exists ssh)) {
    Write-Host "Starting boot2docker ..." -ForegroundColor Yellow
    & boot2docker start
    Write-Host "Setting environment variables ..." -ForegroundColor Yellow
    & boot2docker shellinit | % { $_ -replace "^.*(DOCKER_[A-Z_]+)\s*=\s*('|`")?([^']+)('|`")?$", "`$Env:`$1 = '`$3'" } | iex
    & boot2docker ip | % { Write-Host "Docker host is running on IP $_" -ForegroundColor Green }
  }
}

function Stop-DockerEnvironment {
  if (exists boot2docker) {
    Write-Host "Stopping boot2docker ..." -ForegroundColor Yellow
    & boot2docker stop
  }
}

Export-ModuleMember -Function Start-DockerEnvironment, Stop-DockerEnvironment
~~~

After restarting your Powershell session you can then simply setup your Docker development environment by
typing `Start-DockerEnvironment` or (optionally) shutting down the VM by typing `Stop-DockerEnvironment`.
