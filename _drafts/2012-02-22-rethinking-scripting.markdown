---
author: admin
comments: true
date: 2012-02-22 02:21:25+00:00
layout: post
slug: rethinking-scripting
title: Rethinking Scripting
wordpress_id: 628
categories:
- Code
- Networking
tags:
- backup
- config
- Powershell
- script
- SharpSSH
- SSH
---

I recently wrote a post stating I was beginning learning scripting to aid in my network administration tasks. I had initially stated I was going to use Perl for this. Then I tried to write a script at work, but I'm not allowed to install unauthorized software on my machine, much less multiple machines or servers. We are also a Windows shop, so unfortunately no built-in scripting for me. Or so I thought. I overheard our Windows admin group talking about their Powershell scripts and this got me thinking. Can I use Powershell to write scripts for network device administration? It would make a great platform mostly because its authorized and it's built-in to Windows 7 and Server 2008. I started to do some research and one of the first things I came across was the use of the command line version of PuTTY called PLink to SSH into boxes and then pass commands. I started down this path, but it wasn't long before I began running into problems. Problems either passing commands through PLink or being unable to capture the entire output when running multiple commands. I had to find a better way.

I did some more research and discovered the use of a library called [SharpSSH in Powershell](http://huddledmasses.org/scriptable-ssh-from-powershell/). Now this is what I was looking for. In his post, Joel wrote a few Powershell functions using the SharpSSH library. All I have to do is store the DLLs some place and update the script with their location. The Invoke-SSH function he wrote closely mimics Expect scripts. You run a command and then 'expect' the prompt. This allows the function to know when the command is complete, something I was seriously hurting for using PLink.

I'm a firm believer in not re-inventing the wheel, so what I did was take Joel's functions and use them in a manner that would benefit myself and maybe other network admins. Let me walk you through a quick script I wrote utilizing Joel's functions:

First I want to create my SSH session:

    
    New-SshSession root 192.168.1.1 ## Will be prompted for password
    if (Receive-SSH '%')
    {
    #root prompt
    Write-Host "Logged in as root. Starting CLI."
    $a = Invoke-SSH "cli" '>'
    $rootlogin = 'true'
    }


Now you may ask why I created the if statement to check if I received the % prompt. If you're a Juniper engineer then you would know that % is the shell prompt you receive if you log in as root. I know I hard-coded sending the root username, but in future posts I will share a snippet that will allow for more dynamic entry of username/password combos, and if it happens to be the root user, then we would like to send CLI to start the CLI and enter operational mode. The Invoke-SSH function sends command 'cli' and expects the operational prompt >. I used the variable to call the function because I didn't want it to write any output at this time so I capture it in the variable and sit on it. I also set a variable to true if logged in as root, for use later in the script.

Next I want to download the entire config for my backup folder:

    
    #Invoke-SSH outputs the command(first line), the output, and the expected string line(EOF)
    $a = Invoke-SSH "show config | no-more" '>'
    $a|out-file showconfig.txt
    Write-Host "Config Backup Complete."


Here I use a variable to call the Invoke-SSH function and send the command "show config | no-more". Once the command is done executing we should be back at the operational mode prompt, therefore I expect >. I then use out-file to write the variable to a text file, and write-host to display a little status message.

I then want to cleanly close out the session:

    
    #Extra exit when logged in as root.
    if ($rootlogin='true')
    {
    Write-Host "Exiting CLI."
    Send-SSH exit
    }
    Write-Host "Terminating Session."
    Remove-SshSession #Will send exit.


If I previously set the variable of rootlogin to 'true', then my I need an extra exit to get back to the shell prompt before my final exit closing the session.

This post is not intended to be an introduction to Powershell or scripting in general. There are better places for that. I just wanted to share an example of how you can use Powershell + SharpSSH to make your life a little easier. This script was written for Junos devices, however it could easily be modified for IOS devices, or any device for that matter.This example was a basic, single device, config backup script. I have some great snippets I will be sharing in future posts that can bolt on to make a more powerful script. 

If you want the entire script, including Joel's functions:

    
    ## USING the binaries from:
    ## http://downloads.sourceforge.net/sharpssh/SharpSSH-1.1.1.13.bin.zip
    [void][reflection.assembly]::LoadFrom( (Resolve-Path "D:\Scripts\Libraries\Tamir.SharpSSH.dll") )
    
    ## NOTE: These are bare minimum functions, and only cover ssh, not scp or sftp
    ##       also, if you "expect" something that doesn't get output, you'll be completely stuck.
    ##
    ## As a suggestion, the best way to handle the output is to "expect" your prompt,  and then do
    ## select-string matching on the output that was captured before the prompt.
    
    function New-SshSession {
    Param(
    [string]$UserName
    ,  [string]$HostName
    ,  [string]$RSAKeyFile
    ,  [switch]$Passthru
    )
    if($RSAKeyFile -and (Test-Path $RSAKeyFile)){
    $global:LastSshSession = new-object Tamir.SharpSsh.SshShell `
    $cred.GetNetworkCredential().Domain,
    $cred.GetNetworkCredential().UserName
    $global:LastSshSession.AddIdentityFile( (Resolve-Path $RSAKeyFile) )
    }
    else {
    $cred = $host.UI.PromptForCredential("SSH Login Credentials",
    "Please specify credentials in user@host format",
    "$UserName@$HostName","")
    $global:LastSshSession = new-object Tamir.SharpSsh.SshShell `
    $cred.GetNetworkCredential().Domain,
    $cred.GetNetworkCredential().UserName,
    $cred.GetNetworkCredential().Password
    }
    
    
    $global:LastSshSession.Connect()
    $global:LastSshSession.RemoveTerminalEmulationCharacters = $true
    if($Passthru) {
    return $global:LastSshSession
    }
    }
    
    function Remove-SshSession {
    Param([Tamir.SharpSsh.SshShell]$SshShell=$global:LastSshSession)
    $SshShell.WriteLine( "exit" )
    sleep -milli 500
    if($SshShell.ShellOpened) { Write-Warning "Shell didn't exit cleanly, closing anyway." }
    $SshShell.Close()
    $SshShell = $null
    }
    
    function Invoke-Ssh {
    Param(
    [string]$command
    ,  [regex]$expect ## there ought to be a non-regex parameter set...
    ,  [Tamir.SharpSsh.SshShell]$SshShell=$global:LastSshSession
    )
    
    if($SshShell.ShellOpened) {
    $SshShell.WriteLine( $command )
    if($expect) {
    $SshShell.Expect( $expect ).Split("`n")
    }
    else {
    sleep -milli 500
    $SshShell.Expect().Split("`n")
    }
    }
    else { throw "The ssh shell isn't open!" }
    }
    
    function Send-Ssh {
    Param(
    [string]$command
    ,  [Tamir.SharpSsh.SshShell]$SshShell=$global:LastSshSession
    )
    
    if($SshShell.ShellOpened) {
    $SshShell.WriteLine( $command )
    }
    else { throw "The ssh shell isn't open!" }
    }
    
    
    function Receive-Ssh {
    Param(
    [RegEx]$expect  ## there ought to be a non-regex parameter set...
    ,  [Tamir.SharpSsh.SshShell]$SshShell=$global:LastSshSession
    )
    if($SshShell.ShellOpened) {
    if($expect) {
    $SshShell.Expect( $expect ).Split("`n")
    }
    else {
    sleep -milli 500
    $SshShell.Expect().Split("`n")
    }
    }
    else { throw "The ssh shell isn't open!" }
    }
    
    #Script Start
    New-SshSession root 192.168.1.1 ## Will be prompted for password
    if (Receive-SSH '%')
    {
    #root prompt
    Write-Host "Logged in as root. Starting CLI."
    $a = Invoke-SSH "cli" '>'
    $rootlogin = 'true'
    }
    #Invoke-SSH outputs the command(first line) and the expected string line(EOF)
    $a = Invoke-SSH "show config | no-more" '>'
    $a|out-file showconfig.txt
    Write-Host "Config Backup Complete."
    $b = Invoke-SSH "show version" ">"
    $b|out-file showversion.txt
    Write-Host "Show Version Complete."
    
    #Extra exit when logged in as root.
    if ($rootlogin='true')
    {
    Write-Host "Exiting CLI."
    Send-SSH exit
    }
    Write-Host "Terminating Session."
    Remove-SshSession #Will send exit.
