# Session

SESSION_BUFFER Structure

[Netbios is not supported on Windows Vista, Windows Server 2008, and subsequent versions of the operating system]

The SESSION_BUFFER structure contains information about a local network session. One or more SESSION_BUFFER structures follows a SESSION_HEADER structure when an application specifies the NCBSSTAT command in the ncb_command member of the NCB structure.
Syntax
<br>
typedef struct _SESSION_HEADER {<br>
  UCHAR lsn;<br>
  UCHAR state;<br>
  UCHAR local_name[NCBNAMSZ];<br>
  UCHAR Remote_name[NCBNAMSZ];<br>
  UCHAR rcvs_outstanding;<br>
  UCHAR sends_outstanding;<br>
} SESSION_HEADER, *PSESSION_HEADER;<br>
<br>
Members

lsn

    Specifies the local session number.
state

    Specifies the state of the session. This member can be one of the following values.
    Value	Meaning

    LISTEN_OUTSTANDING

    	

    The session is waiting for a call from a remote computer.

    CALL_PENDING

    	

    The session is attempting to connect to a remote computer.

    SESSION_ESTABLISHED

    	

    The session connected and is able to transfer data.

    HANGUP_PENDING

    	

    The session is being deleted due to a command by the local user.

    HANGUP_COMPLETE

    	

    The session was deleted due to a command by the local user.

    SESSION_ABORTED

    	

    The session was abandoned due to a network or user problem.

     
local_name

    Specifies the 16-byte NetBIOS name on the local computer used for this session.
Remote_name

    Specifies the 16-byte NetBIOS name on the remote computer used for this session.
rcvs_outstanding

    Specifies the number of pending NCBRECV commands.
sends_outstanding

    Specifies the number of pending NCBSEND and NCBCHAINSEND commands.

Requirements

Minimum supported client
	Windows 2000 Professional

Minimum supported server
	Windows 2000 Server

End of client support
	Windows XP

End of server support
	Windows Server 2003

Header
	Nb30.h
See Also

The NetBIOS Interface Overview
NetBIOS Structures
NCB
SESSION_HEADER

 

 

Send comments about this topic to Microsoft

Build date: 7/2/2010


# NCB Struct
NCB Structure

[Netbios is not supported on Windows Vista, Windows Server 2008, and subsequent versions of the operating system]

The NCB structure represents a network control block. It contains information about the command to perform, an optional post routine, an optional event handle, and a pointer to a buffer that is used for messages or other data. A pointer to this structure is passed to the Netbios function.
Syntax
<br>
typedef struct _NCB { <br>
  UCHAR  ncb_command;<br>
  UCHAR  ncb_retcode;<br>
  UCHAR  ncb_lsn;<br>
  UCHAR  ncb_num;<br>
  PUCHAR ncb_buffer;<br>
  WORD   ncb_length;<br>
  UCHAR  ncb_callname[NCBNAMSZ];<br>
  UCHAR  ncb_name[NCBNAMSZ];<br>
  UCHAR  ncb_rto;<br>
  UCHAR  ncb_sto;<br>
  void   (CALLBACK *ncb_post)( struct *NCB);<br>
  UCHAR  ncb_lana_num;<br>
  UCHAR  ncb_cmd_cplt;<br>
  UCHAR  ncb_reserve[X];<br>
  HANDLE ncb_event;<br>
} NCB, *PNCB;<br>
<br>
Members

ncb_command

    Specifies the command code and a flag that indicates whether the NCB structure is processed asynchronously. The most significant bit contains the flag. If the ASYNCH constant is combined with a command code (by using the OR operator), the NCB structure is processed asynchronously. The following command codes are supported.
    Code	Meaning
    NCBACTION	

        Windows Server 2003, Windows XP, Windows 2000, and Windows NT:  Enables extensions to the transport interface. NCBACTION is mapped to TdiAction. When this code is specified, the ncb_buffer member points to a buffer to be filled with an ACTION_HEADER structure, which is optionally followed by data. NCBACTION commands cannot be canceled by using NCBCANCEL. NCBACTION is not a standard NetBIOS 3.0 command.

    NCBADDGRNAME 	

    Adds a group name to the local name table. This name cannot be used by another process on the network as a unique name, but it can be added by anyone as a group name.
    NCBADDNAME	

    Adds a unique name to the local name table. The TDI driver ensures that the name is unique across the network.
    NCBASTAT	

    Retrieves the status of either a local or remote adapter. When this code is specified, the ncb_buffer member points to a buffer to be filled with an ADAPTER_STATUS structure, followed by an array of NAME_BUFFER structures.
    NCBCALL	

    Opens a session with another name.
    NCBCANCEL	

    Cancels a previous pending command.
    NCBCHAINSEND	

    Sends the contents of two data buffers to the specified session partner.
    NCBCHAINSENDNA	

    Sends the contents of two data buffers to the specified session partner and does not wait for acknowledgment.
    NCBDELNAME	

    Deletes a name from the local name table.
    NCBDGRECV	

    Receives a datagram from any name.
    NCBDGRECVBC	

    Receives a broadcast datagram from any name.
    NCBDGSEND	

    Sends datagram to a specified name.
    NCBDGSENDBC	

    Sends a broadcast datagram to every host on the local area network (LAN).
    NCBENUM	

        Windows Server 2003, Windows XP, Windows 2000, and Windows NT:  Enumerates LAN adapter (LANA) numbers. When this code is specified, the ncb_buffer member points to a buffer to be filled with a LANA_ENUM structure. NCBENUM is not a standard NetBIOS 3.0 command. 

    NCBFINDNAME	

    Determines the location of a name on the network. When this code is specified, the ncb_buffer member points to a buffer to be filled with a FIND_NAME_HEADER structure followed by one or more FIND_NAME_BUFFER structures.
    NCBHANGUP	

    Closes a specified session.
    NCBLANSTALERT	

        Windows Server 2003, Windows XP, Windows 2000, and Windows NT:  Notifies the user of LAN failures that last for more than one minute.

    NCBLISTEN	

    Enables a session to be opened with another name (local or remote).
    NCBRECV	

    Receives data from the specified session partner.
    NCBRECVANY	

    Receives data from any session corresponding to a specified name.
    NCBRESET	

    Resets a LAN adapter. An adapter must be reset before it can accept any other NCB command that specifies the same number in the ncb_lana_num member.

    Use the following values to specify how resources are to be freed:

        If ncb_lsn is not 0x00, all resources associated with ncb_lana_num are to be freed.
        If ncb_lsn is 0x00, all resources associated with ncb_lana_num are to be freed, and new resources are to be allocated. The ncb_callname[0] byte specifies the maximum number of sessions, and the ncb_callname[2] byte specifies the maximum number of names. A nonzero value for the ncb_callname[3] byte requests that the application use NAME_NUMBER_1.

    NCBSEND	

    Sends data to the specified session partner.
    NCBSENDNA	

    Sends data to specified session partner and does not wait for acknowledgment.
    NCBSSTAT	

    Retrieves the status of the session. When this value is specified, the ncb_buffer member points to a buffer to be filled with a SESSION_HEADER structure, followed by one or more SESSION_BUFFER structures.
    NCBTRACE	

    Activates or deactivates NCB tracing.

    This command is not supported.
    NCBUNLINK	

    Unlinks the adapter.

    This command is provided for compatibility with earlier versions of NetBIOS. It has no effect in Windows.

     
ncb_retcode

    Specifies the return code. This value is set to NRC_PENDING while an asynchronous operation is in progress. The system returns one of the following values:
    Value	Meaning
    NRC_GOODRET	The operation succeeded.
    NRC_BUFLEN	An illegal buffer length was supplied.
    NRC_ILLCMD	An illegal command was supplied.
    NRC_CMDTMO	The command was timed out.
    NRC_INCOMP	The message was incomplete. The application is to issue another command.
    NRC_BADDR	The buffer address was illegal.
    NRC_SNUMOUT	The session number was out of range.
    NRC_NORES	No resource was available.
    NRC_SCLOSED	The session was closed.
    NRC_CMDCAN	The command was canceled.
    NRC_DUPNAME	A duplicate name existed in the local name table.
    NRC_NAMTFUL	The name table was full.
    NRC_ACTSES	The command finished; the name has active sessions and is no longer registered.
    NRC_LOCTFUL	The local session table was full.
    NRC_REMTFUL	The remote session table was full. The request to open a session was rejected.
    NRC_ILLNN	An illegal name number was specified.
    NRC_NOCALL	The system did not find the name that was called.
    NRC_NOWILD	Wildcards are not permitted in the ncb_name member.
    NRC_INUSE	The name was already in use on the remote adapter.
    NRC_NAMERR	The name was deleted.
    NRC_SABORT	The session ended abnormally.
    NRC_NAMCONF	A name conflict was detected.
    NRC_IFBUSY	The interface was busy.
    NRC_TOOMANY	Too many commands were outstanding; the application can retry the command later.
    NRC_BRIDGE	The ncb_lana_num member did not specify a valid network number.
    NRC_CANOCCR	The command finished while a cancel operation was occurring.
    NRC_CANCEL	The NCBCANCEL command was not valid; the command was not canceled.
    NRC_DUPENV	The name was defined by another local process.
    NRC_ENVNOTDEF	The environment was not defined. A reset command must be issued.
    NRC_OSRESNOTAV	Operating system resources were exhausted. The application can retry the command later.
    NRC_MAXAPPS	The maximum number of applications was exceeded.
    NRC_NOSAPS	No service access points (SAPs) were available for NetBIOS.
    NRC_NORESOURCES	The requested resources were not available.
    NRC_INVADDRESS	The NCB address was not valid.
    NRC_INVDDID	

    The NCB DDID was invalid.

    This return code is not part of the IBM NetBIOS 3.0 specification and is not returned in the NCB structure. Instead, it is returned by the Netbios function.
    NRC_LOCKFAIL	The attempt to lock the user area failed.
    NRC_OPENERR	An error occurred during an open operation being performed by the device driver. This error code is not part of the NetBIOS 3.0 specification.
    NRC_SYSTEM	A system error occurred.
    NRC_PENDING	An asynchronous operation is not yet finished.

     
ncb_lsn

    Identifies the local session number. This number uniquely identifies a session within an environment. This number is returned by the Netbios function after a successful NCBCALL command.
ncb_num

    Specifies the number for the local network name. This number is returned by Netbios after a successful NCBADDNAME or NCBADDGRNAME command. This number, not the name, must be used with all datagram commands and for NCBRECVANY commands.

    The number for NAME_NUMBER_1 is always 0x01. The Netbios function assigns values in the range 0x02 to 0xFE for the remaining names.
ncb_buffer

    Pointer to the message buffer. The buffer must have write access. Its uses are as follows:
    Command	Purpose
    NCBSEND	Contains the message to be sent.
    NCBRECV	Receives the message.
    NCBSSTAT	Receives the requested status information.

     
ncb_length

    Specifies the size, in bytes, of the message buffer. For receive commands, this member is set by the Netbios function to indicate the number of bytes received.

    If the buffer length is incorrect, the Netbios function returns the NRC_BUFLEN error code.
ncb_callname

    Specifies the name of the remote application. Trailing-space characters should be supplied to make the length of the string equal to NCBNAMSZ.
ncb_name

    Specifies the name by which the application is known. Trailing-space characters should be supplied to make the length of the string equal to NCBNAMSZ.
ncb_rto

    Specifies the time-out period for receive operations, in 500-millisecond units, for the session. A value of zero implies no time-out. Specify with the NCBCALL or NCBLISTEN command. Affects subsequent NCBRECV commands.
ncb_sto

    Specifies the time-out period for send operations, in 500-millisecond units, for the session. A value of zero implies no time-out. Specify with the NCBCALL or NCBLISTEN command. Affects subsequent NCBSEND and NCBCHAINSEND commands.
ncb_post

    Specifies the address of the post routine to call when the asynchronous command is completed. The post routine is defined as:

    NCB_POST PostRoutine( PNCB pncb );

    where the pncb parameter is a pointer to the completed NCB structure.
ncb_lana_num

    Specifies the LAN adapter number. This zero-based number corresponds to a particular transport provider using a particular LAN adapter board.
ncb_cmd_cplt

    Specifies the command complete flag. This value is the same as the ncb_retcode member.
ncb_reserve

    Reserved; must be zero.

    The length, X, of the ncb_reserve array is dependent upon the system architecture. For 64-bit systems, the array contains 18 elements. Otherwise, the array contains 10 elements.
ncb_event

    Specifies a handle to an event object that is set to the nonsignaled state when an asynchronous command is accepted, and it is set to the signaled state when the asynchronous command is completed. The event is signaled if the Netbios function returns a nonzero value. Only manual reset events should be used for synchronization. A specified event should not be associated with more than one active asynchronous command.

    The ncb_event member must be zero if the ncb_command member does not have the ASYNCH flag set or if ncb_post is nonzero. Otherwise, Netbios returns the NRC_ILLCMD error code.

Remarks

Using ncb_event to issue asynchronous requests requires fewer system resources than using ncb_post. In addition, when ncb_event is nonzero, the pending request is canceled if the thread terminates before the request is processed. This is not true for asynchronous requests sent using ncb_post.
Requirements

# RFC-1002
4.2.1.1.  HEADER

                        1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |         NAME_TRN_ID           | OPCODE  |   NM_FLAGS  | RCODE |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |          QDCOUNT              |           ANCOUNT             |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |          NSCOUNT              |           ARCOUNT             |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   Field     Description

   NAME_TRN_ID      Transaction ID for Name Service Transaction.
                    Requestor places a unique value for each active
                    transaction.  Responder puts NAME_TRN_ID value
                    from request packet in response packet.

   OPCODE           Packet type code, see table below.

   NM_FLAGS         Flags for operation, see table below.

   RCODE            Result codes of request.  Table of RCODE values
                    for each response packet below.

   QDCOUNT          Unsigned 16 bit integer specifying the number of
                    entries in the question section of a Name

                    Service packet.  Always zero (0) for responses.
                    Must be non-zero for all NetBIOS Name requests.

   ANCOUNT          Unsigned 16 bit integer specifying the number of
                    resource records in the answer section of a Name
                    Service packet.

   NSCOUNT          Unsigned 16 bit integer specifying the number of
                    resource records in the authority section of a
                    Name Service packet.

   ARCOUNT          Unsigned 16 bit integer specifying the number of
                    resource records in the additional records
                    section of a Name Service packet.

   The OPCODE field is defined as:

     0   1   2   3   4
   +---+---+---+---+---+
   | R |    OPCODE     |
   +---+---+---+---+---+




NetBIOS Working Group                                           [Page 8]

 
RFC 1002                                                      March 1987


   Symbol     Bit(s)   Description

   OPCODE        1-4   Operation specifier:
                         0 = query
                         5 = registration
                         6 = release
                         7 = WACK
                         8 = refresh

   R               0   RESPONSE flag:
                         if bit == 0 then request packet
                         if bit == 1 then response packet.

   The NM_FLAGS field is defined as:


     0   1   2   3   4   5   6
   +---+---+---+---+---+---+---+
   |AA |TC |RD |RA | 0 | 0 | B |
   +---+---+---+---+---+---+---+

   Symbol     Bit(s)   Description

   B               6   Broadcast Flag.
                         = 1: packet was broadcast or multicast
                         = 0: unicast

   RA              3   Recursion Available Flag.

                       Only valid in responses from a NetBIOS Name
                       Server -- must be zero in all other
                       responses.

                       If one (1) then the NBNS supports recursive
                       query, registration, and release.

                       If zero (0) then the end-node must iterate
                       for query and challenge for registration.

   RD              2   Recursion Desired Flag.

                       May only be set on a request to a NetBIOS
                       Name Server.

                       The NBNS will copy its state into the
                       response packet.

                       If one (1) the NBNS will iterate on the
                       query, registration, or release.

   TC              1   Truncation Flag.



NetBIOS Working Group                                           [Page 9]

 
RFC 1002                                                      March 1987


                       Set if this message was truncated because the
                       datagram carrying it would be greater than
                       576 bytes in length.  Use TCP to get the
                       information from the NetBIOS Name Server.

   AA              0   Authoritative Answer flag.

                       Must be zero (0) if R flag of OPCODE is zero
                       (0).

                       If R flag is one (1) then if AA is one (1)
                       then the node responding is an authority for
                       the domain name.

                       End nodes responding to queries always set
                       this bit in responses.

4.2.1.2.  QUESTION SECTION

                        1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   /                         QUESTION_NAME                         /
   /                                                               /
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |         QUESTION_TYPE         |        QUESTION_CLASS         |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   Field            Description

   QUESTION_NAME    The compressed name representation of the
                    NetBIOS name for the request.

   QUESTION_TYPE    The type of request.  The values for this field
                    are specified for each request.

   QUESTION_CLASS   The class of the request.  The values for this
                    field are specified for each request.

   QUESTION_TYPE is defined as:

   Symbol      Value   Description:

   NB         0x0020   NetBIOS general Name Service Resource Record
   NBSTAT     0x0021   NetBIOS NODE STATUS Resource Record (See NODE
                       STATUS REQUEST)

   QUESTION_CLASS is defined as:




NetBIOS Working Group                                          [Page 10]

 
RFC 1002                                                      March 1987


   Symbol      Value   Description:

   IN         0x0001   Internet class

4.2.1.3.  RESOURCE RECORD

                        1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   /                            RR_NAME                            /
   /                                                               /
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |           RR_TYPE             |          RR_CLASS             |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                              TTL                              |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |           RDLENGTH            |                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+                               |
   /                                                               /
   /                             RDATA                             /
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   Field            Description

   RR_NAME          The compressed name representation of the
                    NetBIOS name corresponding to this resource
                    record.

   RR_TYPE          Resource record type code

   RR_CLASS         Resource record class code

   TTL              The Time To Live of a the resource record's
                    name.

   RDLENGTH         Unsigned 16 bit integer that specifies the
                    number of bytes in the RDATA field.

   RDATA            RR_CLASS and RR_TYPE dependent field.  Contains
                    the resource information for the NetBIOS name.

   RESOURCE RECORD RR_TYPE field definitions:

   Symbol      Value   Description:

   A          0x0001   IP address Resource Record (See REDIRECT NAME
                       QUERY RESPONSE)
   NS         0x0002   Name Server Resource Record (See REDIRECT



NetBIOS Working Group                                          [Page 11]

 
RFC 1002                                                      March 1987


                       NAME QUERY RESPONSE)
   NULL       0x000A   NULL Resource Record (See WAIT FOR
                       ACKNOWLEDGEMENT RESPONSE)
   NB         0x0020   NetBIOS general Name Service Resource Record
                       (See NB_FLAGS and NB_ADDRESS, below)
   NBSTAT     0x0021   NetBIOS NODE STATUS Resource Record (See NODE
                       STATUS RESPONSE)

   RESOURCE RECORD RR_CLASS field definitions:

   Symbol      Value   Description:

   IN         0x0001   Internet class

   NB_FLAGS field of the RESOURCE RECORD RDATA field for RR_TYPE of
   "NB":

                                             1   1   1   1   1   1
     0   1   2   3   4   5   6   7   8   9   0   1   2   3   4   5
   +---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+
   | G |  ONT  |                RESERVED                           |
   +---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

   Symbol     Bit(s)   Description:

   RESERVED     3-15   Reserved for future use.  Must be zero (0).
   ONT           1,2   Owner Node Type:
                          00 = B node
                          01 = P node
                          10 = M node
                          11 = Reserved for future use
                       For registration requests this is the
                       claimant's type.
                       For responses this is the actual owner's
                       type.

   G               0   Group Name Flag.
                       If one (1) then the RR_NAME is a GROUP
                       NetBIOS name.
                       If zero (0) then the RR_NAME is a UNIQUE
                       NetBIOS name.

   The NB_ADDRESS field of the RESOURCE RECORD RDATA field for
   RR_TYPE of "NB" is the IP address of the name's owner.










NetBIOS Working Group                                          [Page 12]

# Proto
<html><a href="http://www.rhyshaden.com/netbios.htm">proto</a></html>

 
