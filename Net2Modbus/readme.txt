


QQ:925295580
e-mail:925295580@qq.com
Time:201512
author:����ΰ



       SW

   beta---(V0.1)


1��TCP/IP����Э��ջ
.֧��UDP
.֧��TCP
.֧��ICMP

2����������efs�ļ�ϵͳ
.֧��unix��׼�ļ�API
.֧��SDHC��׼������V1.0��V2.0������SDK��-16G����ѹ�������������ֲο���Դ :-)��
.�����ڴ�ռ�ã�����һ��512�ֽڵĸ��ٻ��������ļ�������

3��֧��1-write DS18B20 	�¶ȴ�����
.֧�ֵ������ϸ�ʱ��
.֧��ROM������������Ҷ�ӣ�����һ�����߹ҽӶ���¶ȴ�����
.�����Զ�ת��ΪHTML�ļ�

4��TCP/IPӦ��
.֧��TFTP����ˣ���������ļ����������񡣣��˲�������GITHUB�����Ӳ���TIMEOUT �¼���tftp -i
.֧��NETBIOS����
.֧��һ��TCP�����������ض˿�8080.	���Է���
.֧��һ��TCP�ͻ��ˣ����ض˿�40096	Զ�˶˿�2301 Զ��IP192.168.0.163	���������ݽ���
.֧��һ��UDP�㲥��  ���ض˿�02	    �㲥��ַ255.255.255.255	  �������¶Ȳɼ������ݷ��㲥
.֧��һ��HTTP������ ���ض˿�80  	http:ip  ����֮	  ����2ֻ18B20�¶ȴ�������ʾ�ڼ򵥵�SD������ҳ��

5��ϵͳ�����
Program Size: Code=51264 RO-data=28056 RW-data=1712 ZI-data=55048  

6����������

ipaddr��192, 168, 0, 253-209
netmask��255, 255, 0, 0
gw��192, 168, 0, 20
macaddress��xxxxxxxxxx


tks for GitHUB

	    HW

 stm32f107+DP83848CVV+ds18B20*2+SDHC card (4GB)

7���޸���һ��SDcrad��BUG����һ���ж����д���ˣ�fix�����֧��2GB��4GB���ϵ����ֿ�Ƭ�ˡ�20160122
8��������һ������main.h��
#define MYSELFBOARD
��������ˣ���ô��ʾʹ����ΰ���ҵĿ�����
����������ѡ�����Լ������ǿ���ӡ�
û��ʲô�������𣬿��һ����ֻ��IO�����иĶ���20160122


 20160123
9������һ��SMTPӦ�ã�����ͨ������USED_SMTP��ʹ�� ��
10�����smtp.c����ֲ�Ͳ��ԡ��������ҵ����䷢���ʼ���������Ҫ���ùر�SSL��������Ҫ�޸�һ��Դ�� �ļ����궨�塣
11���Ѳɼ����¶�����20���ӷ�һ���ʼ������Լ��������С���ɡ�  ������ͨ�������IP�������ȥ����ǽ

12��������һ�·������ʱ��Ϊ5����һ���ʼ���@163��������ƴ�Լ��300���ʼ�


20160402

13�������з������ڴ�й¶�����,ͨ�������ڴ����Ϣͨ��TCP����� 2301�˿�debug����
  �쳣������
 ���ڴ������ֽڣ���6144
 �����ڴ棨�ֽڣ���5816
 ʣ���ڴ������ֽڣ���328
 ʹ�ñ�ʾ��1
 ����������
 ���ڴ������ֽڣ���6144
 �����ڴ棨�ֽڣ���20
 ʣ���ڴ������ֽڣ���6124
 ʹ�ñ�ʾ��1
 ��Ȼmemallocʹ�ڴ�������Ҵ�����Ϊ����SMTPӦ�ó���ʹ��malloc������������ʹ�õ������
 ���Կ϶���SMTP������
 ��һ����������ΪSMTP��smtp_send_mail������
 smtp_send_mail_alloced(struct smtp_session *s)
 ����ʹ�õ�
 s = (struct smtp_session *)SMTP_STATE_MALLOC((mem_size_t)mem_len);
 ������һ���ڴ�û�����������ͷš�
 ��������
 �������յ������Ӧ�ô��벻����������һ�������� mem_le��С���ڴ���һֱ������328�ֽڵ�ʣ���ڴ档
 �����յ�������������mem��Ӧ�ó���ȫ����ȡ�����㹻���ڴ�顣�����ֵ��ڴ������
 �������� �ͷŵ��ڴ���  (struct smtp_session *) s
 ���ּ�������

 1����������ֹ	�����ա�
   if (smtp_verify(s->to, s->to_len, 0) != ERR_OK) {
    return ERR_ARG;
  }
  if (smtp_verify(s->from, s->from_len, 0) != ERR_OK) {
    return ERR_ARG;
  }
  if (smtp_verify(s->subject, s->subject_len, 0) != ERR_OK) {
    return ERR_ARG;
  }
  ����û�ж�  smtp_send_mail_alloced ���������ж���������˴����ػ���ɺ�������������ֹ
  Ҳ�ͻᵼ�� (struct smtp_session *) s	û�л����ͷţ���Ϊ�ڲ�������ֹʱ���ں��洦��ģ�
  ���ǿ��ǵ�Դ�����ǹ̶��Ĵ�Ƭ��flash��ȡ�õģ����ּ��ʼ���û�С����Ǵ��ڷ��ա�����ͳһ��Ϊ
  if (smtp_verify(s->to, s->to_len, 0) != ERR_OK) {
    	 err = ERR_ARG;
     goto leave;
  }
  if (smtp_verify(s->from, s->from_len, 0) != ERR_OK) {
    	 err = ERR_ARG;
     goto leave;
  }
  if (smtp_verify(s->subject, s->subject_len, 0) != ERR_OK) {
    	 err = ERR_ARG;
     goto leave;
  }

  2����������TCP���ӣ���Ҫԭ��
  ԭ���ĺ���Ϊ��
  if(tcp_bind(pcb, IP_ADDR_ANY, SMTP_PORT)!=ERR_OK)
	{
	return	ERR_USEl;
  	   
	}
  ��Ȼ����ͬ���Ļ����malloc �����˵���û�б����ã��޸�Ϊ
  if(tcp_bind(pcb, IP_ADDR_ANY,SMTP_PORT)!=ERR_OK)
  {
	err = ERR_USE;
    goto leave;		   
  }

   ����	  leave�оͻ��Զ������ͷŵ������������ֹ�Ķ���ɵ��ڴ��������⡣
   leave:
  smtp_free_struct(s);
  return err;

 ��������һ�����⡣�Ǿ��Ǳ��뱣֤malloc ��free �ɶԳ��֡�



 14��NETBIOS ���ַ���������lwipopts.h������
 #define NETBIOS_LWIP_NAME "WJW-BOARD"
 ��ȷ������
 ��������ʹ�����¸�ʽ�ҵ����ӵ�IP��ַ
 ping 	wjw-board 
 ������ָ��IP��ַ
 20160410



 /*�����з��ֳ�ʱ�����к�SMTP����ֹͣ������������ڴ�����������Ѿ���������波���޸Ľ��н��������������-��17��*/
  20160427

 15���޸�SNMTP��timout��ʱʱ��ͳһΪ2���ӣ���Ϊ�ҵ��ʼ��ط�ʱ��Ϊ3���ӡ�Ĭ�ϵ�10����̫�������޸�֮��������Ӱ��ġ�fix

 /** TCP poll timeout while sending message body, reset after every
 * successful write. 3 minutes def:(3 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT_DATABLOCK  ( 1 * 60 * SMTP_POLL_INTERVAL / 2)
/** TCP poll timeout while waiting for confirmation after sending the body.
 * 10 minutes def:(10 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT_DATATERM   (1 * 60 * SMTP_POLL_INTERVAL / 2)
/** TCP poll timeout while not sending the body.
 * This is somewhat lower than the RFC states (5 minutes for initial, MAIL
 * and RCPT) but still OK for us here.
 * 2 minutes def:( 2 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT            ( 1 * 60 * SMTP_POLL_INTERVAL / 2)
 20160427
 
  16�����Ӽ��SMTP TCP ���ֵı�������
   smtp_Tcp_count[0]//tcp new count 
    smtp_Tcp_count[1]//bind count
	 smtp_Tcp_count[2]connect count 
	 smtp_Tcp_count[3]bind fail save the all pcb list index number
	 that all use debug long time running on smtp .
 20160427

17�����ֲ���SMTP�������ƺ�����������ˣ������޸�����15������ȫ��Ϊ2���� ��30*4*0.5S=1MIN *2=2min 

/** TCP poll interval. Unit is 0.5 sec. */
#define SMTP_POLL_INTERVAL      4
/** TCP poll timeout while sending message body, reset after every
 * successful write. 3 minutes def:(3 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT_DATABLOCK  30*2
/** TCP poll timeout while waiting for confirmation after sending the body.
 * 10 minutes def:(10 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT_DATATERM   30*2
/** TCP poll timeout while not sending the body.
 * This is somewhat lower than the RFC states (5 minutes for initial, MAIL
 * and RCPT) but still OK for us here.
 * 2 minutes def:( 2 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT
20160429            30*2
18���ӳ���KEEPALIVBEʱ��Ϊ
	pcb->so_options |= SOF_KEEPALIVE;
   pcb->keep_idle = 1500+150;// ms
    pcb->keep_intvl = 1500+150;// ms
   pcb->keep_cnt = 2;// report error after 2 KA without response
 20160429


19�����Ӽ���SMTP����ı���	smtp_Tcp_count[10]	upsize 10 dword

20�����Ӽ��SMTP TCP ���ֵı�������
   smtp_Tcp_count[0]//tcp new count 
    smtp_Tcp_count[1]//bind count
	 smtp_Tcp_count[2]connect count 
	 smtp_Tcp_count[3]bind fail save the all pcb list index number
add smtp send result 
              smtp_Tcp_count[4]|= (smtp_result);  
			  smtp_Tcp_count[4]|= (srv_err<<8);
			  smtp_Tcp_count[4]|= (err<<24);

			  if(err==ERR_OK){smtp_Tcp_count[5]++;}	//smtp�ɹ�����ͳ��

			   if(arg!=(void*)0)
			   {
				smtp_Tcp_count[6]=0xAAAAAAAA ;	 //�в���
			   }
			   else
			   {
			   
			   smtp_Tcp_count[6]=0x55555555 ;	//û�в���
			   }
20160430
21��

 ���ϲ����з������е�9�����Ҿͻ᲻��ִ��SMTP���뷵���������£� ���ֽ���ǰ--���ֽ��ں�
 ��Receive from 192.168.0.253 : 40096����
5D 11 00 00     5D 11 00 00    58 11 00 00    00 00 00 00     04 00 00 F6  48 11 00 00  55 55 55 55 

��������ݿ�֪��
tcp new count=0x115d
bind count=0x115d
connect count=0x1158
bind fail  number=0
smtp_result=  4��SMTP_RESULT_ERR_CLOSED��
srv_err=00
tcp err=0xf6�Ǹ�����ҪNOT +1=(-10) �������Ϊ  ERR_ABRT	  Connection aborted

�������ݶ��񣬲��ڱ仯��˵�������TCP baind û�й�ϵ����TCP new��֮ǰ�ĵ������⣬���Լ���������������ҡ�

20160513

22�����ٶȵ��죬10����һ��SMTP ���ӡ��޸�SMTP Ӧ�ó���ĳ�ʱʱ��Ϊ8����ͬʱ����
smtp_Tcp_count[8]++;�������ܵĵ���SMTP�Ĵ���
smtp_Tcp_count[7]++;������SMTP �õ�TCP new֮ǰ�Ĵ������ų�һ��TCP new�����⣡
����������һֱ�仯�������û�б仯��֤��TCP ��new������֮����ǰ�ƣ�ֱ���������ĵط�һ����ų���

����׷�����ֹͣTCP ���ӵ����⡣
20160513


 /********Ϊ�˽ӿڳ��µ�485����*******************/

23������modbus RTU �������ֵײ�TXRX�Ĵ��룬����ʹ��RTU ��TCP ����͸��485.��߲�����ֻת����
������һ����

//��������ʹ��MODBUS RTU TX/RX�ײ��շ� (ע��Ӧ�ò�û��ʹ�á���ΪӦ�ò���㽻��������������߽�����RTU͸��)
#define USED_MODBUS

18:52����TXͨ����������TIM3��USART2��Remap

20160613

24������STM32�Ĺ̼��⣬ʹ��2011�汾�ģ�ԭ����ԭ����2009�汾��CLϵ�еĴ������������⡣�����ʲ���������Ϊ2011�������ˣ�
    MODBUS RTU�����������޸ġ��������ε�CRCУ��Ĳ�������ֱ��͸����
	ע����MODBUS ������ڴ�����Ͽ�Ҳ�ǰ�˫����	��
20160614

25�������24���������ս���Ǿ������⵼�µģ��͹̼���û�й�ϵ��


#if !defined  HSE_VALUE
 #ifdef STM32F10X_CL   
  #define HSE_VALUE    ((uint32_t)8000000) /*!< Value of the External oscillator in Hz */
 #else 
  #define HSE_VALUE    ((uint32_t)8000000) /*!< Value of the External oscillator in Hz */
 #endif /* STM32F10X_CL */
#endif /* HSE_VALUE */


  ����	 #define HSE_VALUE    ((uint32_t)8000000) /*!< Value of the External oscillator in Hz */
  Ҫ����Ϊ���Լ����ⲿ����ֵ��
20160615

26�������˷������ӿںͳ��£���һ��Ҫ׼�����ڴ������ݲɼ��������ܣ����Զ��˼���Э�飬���ֱ��ת����

 IP4_ADDR(&ip_addr,114,215,155,179 );//���·�����

 /*���������µ�Э������ 
Э��֡��ʽ
֡ͷ+����+������+\r\n
֡ͷ��
һ֡���غͷ����������Ŀ�ʼͬʱ�������ж������ϴ������·�������
��XFF+0X55������ʾ�����ϴ���������
��0XFF+0XAA��: ��ʾ�������·�������
���ͣ�
0x01:��ʾ������ʪ�ȴ�����
0x02��ʾ���մ�����
0x03 ��ʾPH ֵ������
0x04 ��ʾ��������������
������
��ͬ�����͵Ĵ�������������������ǳ����ṩ��MODBUS-RTU��Э��ֱ�Ӵ����ϡ�

 ���������ͣ�FF AA + ���� +��modbus RTU ������ ��+\r\n
 ���ػظ�  ��FF 55 + ���� +��modbus RTU ������ ��+\r\n

*/

 27�����ݷ�����Ҫ���޸����ص��ϱ�����

 getid �����Ϊ

 AB +00 +[���ֽ�ID]+CD +'$'+'\r'+'\n'   9���ֽ�

 �޸��ϱ�����λ

 FF 55 + ���ͣ�1�ֽڣ�+ID��3�ֽڣ�+[modbus-RTU������] + '$'+'\r'+'\n'

20180710

28������DHCP 
20160710

29���޸��˷������ӿں���Ҫ���޸���IP��ַ�������䡣
 IP4_ADDR(&ip_addr,60,211,192,14);//���������ͻ���IP��������ַ��
20160920


30����stm32f10x.h������UID�ĵ�ַӳ�䣬�Ա�ֱ�ӻ�ȡID��

 typedef struct
 {
 	uint32_t UID0_31;
 	uint32_t UID32_63;
	uint32_t UID64_96;
 }ID_TypeDef;
#define UID_BASE              ((uint32_t)0x1FFFF7E8) /*uid 96 by wang jun wei */
#define UID    ((ID_TypeDef*) UID_BASE)
20160920
31���޸�ID��CPU��UID��������
  uint32_t id;

  UID_STM32[0]=0xAB;//mark

  UID_STM32[1]=0x00;//����

  id=(UID->UID0_31+UID->UID32_63+UID->UID64_95);//sum ID
  
  UID_STM32[2]=id;
  UID_STM32[3]=id>>8;//
  UID_STM32[4]=id>>16; // build ID 	lost The Hight 8bit data save the low 3 byte data as it ID

  UID_STM32[5]=0xCD; //mark

  UID_STM32[6]=0x24;
  UID_STM32[7]=0x0d;  //efl
  UID_STM32[8]=0x0a;
 20160920
32������MAC��ַ��оƬID��ȡ��
 void GetMacAddr(void)
{

uint32_t cid;
cid=(UID->UID0_31+UID->UID32_63+UID->UID64_95);//sum ID
macaddress[0]=0x00;
macaddress[1]=0x26;
macaddress[2]=cid;
macaddress[3]=cid>>8;
macaddress[4]=cid>>16;
macaddress[5]=cid>>24;

}
20160921
33���޸�MODBUS�ϱ��Ĵ���ID

	 Mb2TcpBuff[0]=0xFF;
	 Mb2TcpBuff[1]=0x55;
	 Mb2TcpBuff[2]=Statues_MB;
	 Mb2TcpBuff[3]=UID_STM32[2];
	 Mb2TcpBuff[4]=UID_STM32[3];
	 Mb2TcpBuff[5]=UID_STM32[4];
	 memcpy(&Mb2TcpBuff[6],ucMBFrame,Mb2TcpLenth);//���ݸ��ƽ�TCP����buf��
	 Mb2TcpBuff[Mb2TcpLenth+6]=0x24;
	 Mb2TcpBuff[Mb2TcpLenth+6+1]=0x0d;
	 Mb2TcpBuff[Mb2TcpLenth+6+2]=0x0a;	
20160921 


###########################################################################################

     ��Э��

##########################################################################################

34���ڳ�����ƽ̨���޸�ͨѶЭ��Ϊ

֡ͷ/ͨ�ŷ���/Ӧ����/����ID/�豸����/�豸����/�豸id/����/֡β/�����

֡ͷ��2�ֽڣ�FFAA
ͨ�ŷ���1�ֽڣ�01���ص���������02������������
��ϵͳId��ҵ����ϵͳ��ID��
����ID��3���ֽڣ��豸���ò���
�豸���ࣺ1���ֽ� 01���ء�02�豸��03ֱ�� д��
�豸���ͣ�2���ֽڡ��豸���ò���
�豸id��3���ֽڡ��豸���ò�����ֱ���豸id�������ֽ�
���ݣ�MODBUS-RTU���Զ���
֡β��2�ֽ� AAFF
�������3�ֽ� 240D0A



  0-1+FFAA (֡ͷ)
  2+ͨ�ŷ���(01:���ص�������02:������������)
  3+��ϵͳId��ҵ����ϵͳ��ID��
  456+����ID(3���ֽڣ��豸���ò���)
  7+�豸���ࣨ1���ֽ� 01���ء�02�豸��03ֱ�� д����
  89+�豸���ͣ�2���ֽڡ��豸���ò�����
  101112+�豸id��3���ֽڡ��豸���ò�����ֱ���豸id�������ֽڣ�
  13-n+���ݣ�MODBUS-RTU���Զ���
  n+1+2+֡β��2�ֽ� AAFF
  n+3+4+5�������3�ֽ� 240D0A

  ����IDΪ010203�ķ�����������Э��Ϊ
  FF AA + 02 + 01 +  01  02 03 + 01 + 00 00 +  00   00   00 + MODBUS-RTU	 +	AA    FF +   24     0D     0A
  [0][1]  [2]  [3]	[4] [5] [6]	 [7]  [8][9]  [10] [11]	[12]  [13]~[n]			[n+1] [n+2]	 [n+3] [n+3] [n+4]







  ����IDΪ010203�����ص�������Э��Ϊ
  FF AA +   01 + 01  +   01  02 03    +   01    + 00 00     +  00   00   00     + MODBUS-RTU	 +	 AA    FF +   24     0D     0A
  [0][1]   [2]  [3]	    [4] [5] [6]	      [7]     [8][9]       [10] [11][12]      [13]~[n]			[n+1] [n+2]	 [n+3] [n+4] [n+5]

   FFAA       [0][1] 
   01	      [2]
   01 	      [3]
   01 02 03	  [4][5][6]       
   01         [7]
   00 00      [8][9]
   000000     [10][11][12] 
   MODBUS-RTU [13]~[n]
   AAFF 	  [n+1][n+2]
   240D0A	  [n+3][n+4][n+5]







 Mb2TcpLenth=usLength;//���½��ܵ������ݳ��ȣ�׼������TCP����
 Mb2TcpBuff[0]=0xFF;
 Mb2TcpBuff[1]=0xAA;          //֡ͷ

 Mb2TcpBuff[2]=0x01;         //�ϴ����ݵ���������ʾ

 Mb2TcpBuff[3]=0x01;         //��ϵͳId��ҵ����ϵͳ��ID�� ,z��ʱδ�ã�ֵĬ��Ϊ01����


 Mb2TcpBuff[4]=UID_STM32[2];
 Mb2TcpBuff[5]=UID_STM32[3];
 Mb2TcpBuff[6]=UID_STM32[4]; //����ID(3���ֽڣ��豸���ò���)

 Mb2TcpBuff[7]=0x01;         //�豸���ࣨ1���ֽ� 01���ء�02�豸��03ֱ�� д����


 Mb2TcpBuff[8]=0x01;					  
 Mb2TcpBuff[9]=0x01;         //�豸���ͣ�2���ֽڡ��豸���ò�����


 Mb2TcpBuff[10]=UID_STM32[2];
 Mb2TcpBuff[11]=UID_STM32[3];
 Mb2TcpBuff[12]=UID_STM32[4]; //�豸id(3���ֽڣ��豸���ò���),

memcpy(&Mb2TcpBuff[13],ucMBFrame,Mb2TcpLenth);//MODBUS-RTU���ݸ��ƽ�TCP����buf��


 Mb2TcpBuff[Mb2TcpLenth+13+0]=0xAA;
 Mb2TcpBuff[Mb2TcpLenth+13+1]=0xFF;	 //֡β

 Mb2TcpBuff[Mb2TcpLenth+13+2]=0x24;	
 Mb2TcpBuff[Mb2TcpLenth+13+3]=0x0D;
 Mb2TcpBuff[Mb2TcpLenth+13+4]=0x0A;	//�����	
  
 Mb2TcpLenth+=18;//����֡�ܳ��ȣ�������Ϊ���ͱ�ʾ���͵�TCP buf


     	     	                        			 	 





  
֡ͷ/ͨ�ŷ���/Ӧ����/����ID/�豸����/�豸����/�豸id/����/֡β/�����

֡ͷ��2�ֽڣ�FFAA
ͨ�ŷ���1�ֽڣ�01���ص���������02������������
��ϵͳId��ҵ����ϵͳ��ID��
����ID��3���ֽڣ��豸���ò���
�豸���ࣺ1���ֽ� 01���ء�02�豸��03ֱ�� д��
�豸���ͣ�2���ֽڡ��豸���ò���
�豸id��3���ֽڡ��豸���ò�����ֱ���豸id�������ֽ�
���ݣ�MODBUS-RTU���Զ���
֡β��2�ֽ� AAFF
�������3�ֽ� 240D0A

ֱ���豸Э���У�����id������000000

����id 
FFAA(֡ͷ2�ֽ�) 01(ͨ�ŷ���1�ֽ�) 01(��ϵͳId1�ֽ�) 000001(����ID3�ֽ�) 01(�豸����1�ֽ�) 0001(�豸����2�ֽ�) 000000(�豸id 3�ֽ�) 00(����N�ֽ�) AAFF(֡β2�ֽ�) 240D0A(�����3�ֽ�) 

FFAA 01 01 000002 01 0001 000000 00 AAFF 240D0A 

�����ϱ��豸��Ϣ

FFAA 01 01 000001 02 0001 000001 FE030605DC09EC00C876F8 AAFF 240D0A
FFAA 01 01 000001 02 0001 000001 FE030605DC09EC00C876F8 AAFF 240D0A
FFAA 01 01 000001 02 0001 000002 FF030605DC09EC00C876F8 AAFF 240D0A
FFAA 01 01 000001 02 0001 000003 FE030605DC09EC00C876F8 AAFF 240D0A

ֱ��
FFAA 01 01 000000 03 0001 000001 FE030605DC09EC00C876F8 AAFF 240D0A
FFAA 01 01 000000 03 0001 000002 FE030605DC09EC00C876F8 AAFF 240D0A
FFAA 01 01 000000 03 0001 000003 FE030605DC09EC00C876F8 AAFF 240D0A


getid
FF AA 01 01 9D A1 1A 01 00 01 9D A1 1A 00 AA FF 24 0D 0A


000000-Tx:01 03 00 00 00 0E C4 0E
000002-Tx:03 03 00 00 00 0E C5 EC


 FFAA 01 01 000001 02 0001 000003 01030000000EC40E AAFF 240D0A

�����ϱ����ݣ�

FF AA 01 01 00 00 00 01 01 01 00 00 00 1C FC 00 00 00 00 E0 00 00 00 E0 00 AA FF 24 0D 0A



35���޸�keepaliveʱ��Ϊ1000MS 500ms���̫С������̫Ƶ����	  20170914

36���޸��ϲ�ͳһ�·����ݱ��Ľ�ȡ����Ϊǰ���ֽڣ��ϱ����䣬getid ����

�ϲ�ͳһ�·��� FFAA 02 011000000001020000A650 AAFF 240D0A

����������޸Ĵ���Ϊ��

	char *ptr=(char*)p->payload;
	uint16_t len;
	ptr+=3;	//jump FF AA and other there the ptr is point to MODBUS-RTU   
	if(p->tot_len<1500) //asseter the buffer lenth 
	{
	
	TcpRecvLenth=p->tot_len-5-3;//upadta the recive data lenth	 AA FF  24 0D 0A =5 BYTES
							   
	memcpy(TcpRecvBuf,ptr,TcpRecvLenth); // data copy to appliction data buffer 



 ���ӣ�
 TCP SEVER :FFAA 02 01030000000EC40E AAFF 240D0A
 REPO: FF AA 00 00 00 00 00 02 00 00 00 00 00 01 03 1C 00 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 00 01 00 01 00 01 1F 27 AA FF 24 0D 0A  

  20170915


 37������Э��
 ͨ��Э�����
  
-------------------���ء�ֱ���豸Э��-------------------
֡ͷ/ͨ�ŷ���/Ӧ����/����ID/�豸����/�豸����/�豸id/����/֡β/�����
 
֡ͷ��2�ֽڣ�FFAA
ͨ�ŷ���1�ֽڣ�01���ص���������02������������
��ϵͳId��1�ֽڣ�ҵ����ϵͳ��ID��
����ID��3���ֽڣ��豸���ò���
�豸���ࣺ1���ֽ� 01����(�����ϱ�idʱ��01)��02�豸(�����ϱ��豸������02)��03ֱ�� д��
�豸���ͣ�2���ֽڡ��豸���ò���
�豸id��3���ֽڡ��豸���ò�����ֱ���豸id�������ֽ�
���ݣ�MODBUS-RTU���Զ���
֡β��2�ֽ� AAFF
�������3�ֽ� 240D0A

ֱ���豸Э���У�����id������000000
����id 
FFAA 01 01 000001 01 0001 000000(�豸id��������) 00��������00���棩 AAFF 240D0A 
�����ϱ��豸��Ϣ
FFAA 01 01 000001 02 0001 000001 FE030605DC09EC00C876F8 AAFF 240D0A
ֱ��
FFAA 01 01 000000 03 0001 000001 FE030605DC09EC00C876F8 AAFF 240D0A

 
 -------------------�ͻ���IDЭ��-------------------
֡ͷ��2�ֽڣ�FFAA
ͨ�ŷ���1�ֽڣ�01���ص���������02������������
��ϵͳId��1�ֽ� ҵ����ϵͳ��ID��
�豸���ͣ�1�ֽ� ���ػ���ֱ���豸00,01
app�Ƿ�������ӷ���00��������app��Ҫ��app��Ӧ���û��˺Ŷ�Ӧ����������idһ�η��͹�����01������Ϣ
Client Id��3���ֽ�,��ҳ������ֽ�
Client ���ͣ�01 android��02 ios��03 web
����orֱ���豸�ģ�id�����ν�������û�У�������Ϣʱ��
���ݣ�MODBUS-RTU���Զ��壬���ν�������Ϊ00
֡β��2�ֽ� AAFF
�������3�ֽ� 240D0A
App Client ID
FFAA 02 01 00 00 000001 01 AAFF 240D0A 240D0A 
FFAA 02 01 00 00 000001 01 000001000002000003 AAFF 240D0A
FFAA 02 01 00 00 0000000001 03 000001 AAFF ��ҳ�������Ӳ����ָ���
 
 -------------------�ͻ��˷�����������Э��-------------------
֡ͷ��2�ֽڣ�FFAA
ͨ�ŷ���1�ֽڣ�01���ص���������02������������
��ϵͳId��1�ֽ� ҵ����ϵͳ��ID��
�豸���ͣ�1�ֽ� ���ػ���ֱ���豸00,01
app�Ƿ�������ӷ���00��������app��Ҫ��app��Ӧ���û��˺Ŷ�Ӧ����������idһ�η��͹�����01������Ϣ
Client ���ͣ�01 android��02 ios��03 web
Client Id��3���ֽ�,��ҳ������ֽ�
����orֱ���豸�ģ�id�����ν�������û�У�������Ϣʱ��
���ݣ�MODBUS-RTU���Զ��壬���ν�������Ϊ00
֡β��2�ֽ� AAFF
�������3�ֽ� 240D0A
App Client Data
FFAA 02 01 00 01 01 000001  000001 FE030605DC09EC00C876F8 AAFF 240D0A
FFAA 02 01 00 01 03 0000000001  000001 FE030605DC09EC00C876F8 AAFF 240D0A��ҳ�������Ӳ����ָ���
 
 -------------------�ͻ��˷���������Э��-------------------
FFAA 02 011000000001020000A650 AAFF 240D0A

20170915

