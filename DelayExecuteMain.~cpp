//---------------------------------------------------------------------------

#include <vcl.h>
#pragma hdrstop

#include "DelayExecuteMain.h"
//---------------------------------------------------------------------------
#pragma package(smart_init)
#pragma resource "*.dfm"
TformMain *formMain;
//---------------------------------------------------------------------------
__fastcall TformMain::TformMain(TComponent* Owner)
  : TForm(Owner)
{
}
//---------------------------------------------------------------------------
void __fastcall TformMain::FormShow(TObject *Sender)
{
 timerStart->Enabled = true;


}
//---------------------------------------------------------------------------

void __fastcall TformMain::timerStartTimer(TObject *Sender)
{
  timerStart->Enabled = false;
  int nDelay;
  try
  {
    nDelay = StrToInt( ParamStr(1) );
  }
  catch(...)
  {
    Application->Terminate();
    return;
  }
  AnsiString asCmd = ParamStr(2);
  AnsiString asParam = "";
  for( int i=3; i<=ParamCount(); i++ )
    if( asParam.IsEmpty() )
       asParam = ParamStr(i);
    else
      asParam = asParam + " " + ParamStr(i);


   labelExecuteCmd->Caption = asCmd + " " + asParam;
   labelDelaySeconds->Caption = IntToStr(nDelay) + " second(s)";

   int nCurrDelay = 0;
   const int nQuant = 1;

   while( nCurrDelay < nDelay && ! Application->Terminated )
   {
     labelDelaySeconds->Caption = IntToStr( nDelay - nCurrDelay ) + " seconds left";
     Application->ProcessMessages();
     ::Sleep( nQuant * 1000 );
     nCurrDelay += nQuant;

   }

   if(  Application->Terminated )
     return;

  STARTUPINFO si = { sizeof( STARTUPINFO ) };
    ZeroMemory( &si, sizeof( STARTUPINFO ) );
    si.cb = sizeof( STARTUPINFO );
    si.dwFlags = STARTF_USESHOWWINDOW;
    si.wShowWindow = SW_SHOW;

    PROCESS_INFORMATION pi;
    ZeroMemory( &pi, sizeof(pi) );


    CreateProcess( asCmd.c_str(),
                 asParam.c_str(),
                 NULL,NULL, //&SecAttrs, &SecAttrs,
                 TRUE,
                 NORMAL_PRIORITY_CLASS	,
                 NULL, NULL,
                 &si, &pi);

    Application->Terminate();

}
//---------------------------------------------------------------------------

