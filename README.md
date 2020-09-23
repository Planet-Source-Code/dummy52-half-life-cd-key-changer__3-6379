<div align="center">

## Half\-Life CD\-KEY Changer


</div>

### Description

The purpose of this code is to demonstrate the Windows API registry functions. Using these functions, I put together a short program for fellow Half-Life gamers to easily change their cd-key without editing the registry via regedit.
 
### More Info
 
The program paramaters are optional. The paramaters make it faster to change your cdkey, and are used as: <program>.exe <new-cdkey>


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Dummy52](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/dummy52.md)
**Level**          |Beginner
**User Rating**    |4.7 (14 globes from 3 users)
**Compatibility**  |C, C\+\+ \(general\), Microsoft Visual C\+\+, Borland C\+\+, UNIX C\+\+
**Category**       |[Registry](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/registry__3-11.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/dummy52-half-life-cd-key-changer__3-6379/archive/master.zip)





### Source Code

```
/*************************************************
 * Half-Life CD-KEY Changer
 * Author: Devin McLean
 * Contact via IRC: GamesNET #team-de
*************************************************/
#include <stdio.h>
#include <windows.h>
int main(int argc, char *argv[])
{
	HKEY hKey;
	LONG lRet;
	unsigned char dwData[14];
	char newKey[14];
	DWORD dwSize = 14;
	memset(newKey,0,14);
	if (argc > 2)
	{
		printf("\nToo many params.\n");
		return 0;
	}
	else if (argc == 2)
	{
		if (strlen(argv[1]) != 13)
		{
			printf("\nInvalid Half-Life CD-KEY!\n");
			return 0;
		}
		strcpy(newKey,argv[1]);
	}
	if ((lRet = RegOpenKeyEx(HKEY_CURRENT_USER, "Software\\Valve\\Half-Life\\Settings", 0, KEY_ALL_ACCESS, &hKey)) != ERROR_SUCCESS)
	{
		printf("\nCould not find Half-Life installed!\n");
		return 0;
	}
	if ((lRet = RegQueryValueEx(hKey, "Key", NULL, 0, dwData, &dwSize)) != ERROR_SUCCESS)
	{
		RegCloseKey(hKey);
		printf("\nCould not find Half-Life CD-KEY!\n");
		return 0;
	}
	printf("\nCurrent Half-Life CD-KEY: %s\n",dwData);
	if (strlen(newKey) == 0)
	{
		printf("New Half-Life CD-KEY: ");
		scanf("%s",newKey);
		if (strlen(newKey) != 13)
		{
			printf("\nInvalid Half-life CD-KEY!\n");
			RegCloseKey(hKey);
			return 0;
		}
		fflush(stdin);
	}
	if ((lRet = RegSetValueEx(hKey, "Key", 0, REG_SZ, (LPBYTE) newKey, dwSize)) != ERROR_SUCCESS)
	{
		RegCloseKey(hKey);
		printf("\nCould not set Half-Life CD-KEY!\n");
		return 0;
	}
	RegCloseKey(hKey);
	printf("\nHalf-Life CD-KEY successfully changed to: %s\n",newKey);
	return 0;
}
```

