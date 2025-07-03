foreach($Adapter in Get-NetAdapter)
{
    New-NetIPAddress -IPAddress 135.181.16.180 -DefaultGateway 135.181.16.1 -PrefixLength 24 -InterfaceIndex $Adapter.InterfaceIndex
}
