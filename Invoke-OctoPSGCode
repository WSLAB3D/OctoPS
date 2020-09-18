##### WORK IN PROGRESS. using existing function as a template
function Invoke-OctoPSGcode {
    <#
    .SYNOPSIS
        Send any G-Code command to an OctoPrint server.
    .DESCRIPTION
        Send any G-Code command to an OctoPrint server.
    .EXAMPLE
        PS C:\> Invoke-OctoPSGcode -id 1 -SkipCertificateCheck -gcode {G0 Z1.5}
        Sent the G-Code G0 Z1.5 (Move print head to +1.5mm)
    .INPUTS
        !!! UPDATE !!!
    #>
    [CmdletBinding()]
    param (
        # OctoPrint Host  Id
        [Parameter(Mandatory = $False,
            Position = 0,
            ValueFromPipelineByPropertyName = $true)]
        [int32[]]
        $Id = @(),

        # Skips certificate validation checks. This includes all validations such as expiration, revocation, trusted root authority, etc.
        [Parameter(Mandatory = $false)]
        [switch]
        $SkipCertificateCheck=$true,

        # Gcode to send to the printer.
        [Parameter(Mandatory = $true)]
        $gcode
    )
    
    begin {
    }
    
    process {
        if ($Id.count -gt 0) {
            $PHosts = Get-OctoPSHost -Id $Id
        }
        else {
            $PHosts = Get-OctoPSHost | Select-Object -First 1
        }
        foreach ($h in $PHosts) {
            $RestMethodParams = @{
                'Method'        = "Post"
            }
            $RestMethodParams.Add('URI',"$($h.Uri)/api/printer/command")
            $RestMethodParams.Add('Headers',@{'X-Api-Key' = $h.ApiKey})
            $RestMethodParams.Add('ContentType','application/json')

            if ($SkipCertificateCheck)
            {
                $RestMethodParams.Add('SkipCertificateCheck', $SkipCertificateCheck)
            }
            $Body = New-Object System.Collections.Specialized.OrderedDictionary
            Write-Verbose -Message "Setting bed to temperature $($TargetTemp) Celcius."
            $Body.Add("command", $gcode)
                
            $RestMethodParams.Add('Body',(ConvertTo-Json -InputObject $body))
            Invoke-RestMethod @RestMethodParams | Out-Null
        }
    }

    end {
    }
}
