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

 
