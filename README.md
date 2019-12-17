#include <a_samp>

#define FILTERSCRIPT
#if defined FILTERSCRIPT

#define COLOR_LIGHTBLUE 0x33CCFFAA

#define SCM SendClientMessage

new PlayerPressedJump[MAX_PLAYERS];

public OnFilterScriptInit()
{
        print("\n--------------------------------------");
        print(" Yazan ve tasarlayan ; Hexe");
        print("--------------------------------------\n");
        return 1;
}
public OnPlayerConnect(playerid)
{
        PlayerPressedJump[playerid] = 0; // İlk bağlandığında değişkeni 0 olarak ayarlar
        return 1;
}
forward PressJump(playerid);
public PressJump(playerid)
{
    PlayerPressedJump[playerid] = 0; // Değişkeni sıfırlar.!
    ClearAnimations(playerid);
    return 1;
}
forward PressJumpReset(playerid);
public PressJumpReset(playerid)
{
    PlayerPressedJump[playerid] = 0; // Değişkeni sıfırladı!
    return 1;
}
public OnPlayerKeyStateChange(playerid, newkeys, oldkeys)
{
        if((newkeys & KEY_JUMP) && !IsPlayerInAnyVehicle(playerid))
    {
        PlayerPressedJump[playerid] ++;
        SetTimerEx("PressJumpReset", 3000, false, "i", playerid); // Space tuşunu spam yapmazlarsa 10 saniyede bir tekarlar.

        if(PlayerPressedJump[playerid] == 3) // Bir yerden düştüğünüzü var sayın, düşerken kaç kez Space'ye basılsın, çünkü bug yapabiliyorlar isteersen çıkart!
        {
            ApplyAnimation(playerid, "PED", "IDLE_tired", 4.1, 0, 1, 1, 1, 0, 1); // İstersen değiştir fakat, ben Fallover animasyonuna ayarladım!
            SetTimerEx("PressJump", 4500, false, "i", playerid); // Animasyonun ne kadar sürdüğünü gösteren zamanlayıcı fonksiyon!
        }
    }
    return 1;
}
#endif

