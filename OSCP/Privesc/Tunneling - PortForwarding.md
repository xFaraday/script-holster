## SSH portfowarding 

local machine has an internal port that needs to be accessed by external tools like 3306 or 80
`ssh -L victimPORT:127.0.0.1:attackerPORT user@IP`
The victimPORT will now be able to be accessed on the attackermachine locally via the attackerPORT



