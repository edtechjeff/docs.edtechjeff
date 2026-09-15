# Used to Pull the logical layout of your Active Directory

```powershell
function Show-ADTree {
    [CmdletBinding()]
    param (
        [string]$SearchBase = (Get-ADDomain).DistinguishedName
    )

    Import-Module ActiveDirectory

    function Get-OUTree {
        param (
            [string]$Base,
            [string]$Prefix = ""
        )

        $Children = @(
            Get-ADOrganizationalUnit `
                -Filter * `
                -SearchBase $Base `
                -SearchScope OneLevel |
            Sort-Object Name
        )

        for ($i = 0; $i -lt $Children.Count; $i++) {

            $OU = $Children[$i]
            $IsLast = ($i -eq ($Children.Count - 1))

            if ($IsLast) {
                $Connector = "└── "
                $ChildPrefix = "$Prefix    "
            }
            else {
                $Connector = "├── "
                $ChildPrefix = "$Prefix│   "
            }

            Write-Host "$Prefix$Connector$($OU.Name)"

            # Recursively display child OUs
            Get-OUTree `
                -Base $OU.DistinguishedName `
                -Prefix $ChildPrefix
        }
    }

    $Domain = Get-ADDomain

    Write-Host ""
    Write-Host $Domain.DNSRoot
    Get-OUTree -Base $SearchBase
    Write-Host ""
}
```

- Run 

```powershell
show-adtree
```