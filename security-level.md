# The Security Level Claim (security-level)

The term "entity" here is as defined by Entity Attestation Token (EAT).

This claim characterizes the design intent of the entity's ability to defend against attacks aimed at capturing the signing key, forging claims and forging EATs.

This claim is only to give the recipient a rough idea of the security design the entity is aiming for.
This is via a simple, non-extensible set of three levels.

While this claim may be forwarded in Attestation Results as described in {{relationship}}, this claim MUST NOT be used to represent the output of a RATS Verifier.

This takes a broad view of the range of defenses because EAT is targeted at a broad range of use cases.
The least secure level may have only minimal SW defenses.
The most secure level may have specialized hardware to defend against hardware-based attacks.

Only through expansive certification programs like Common Criteria is it possible to sharply define security levels.
Sharp definition of security levels is not possible here because the IETF doesn't define and operate certification programs.
It is also not possible here because any sharp definition of security levels would be a document larger than the EAT specification.
Thus, this definition takes the view that the security level definition possible is a simple, modest, rough characterization.

1 - Unrestricted:
: An entity is categorized as unrestricted when it doesn't meet the criteria for any of the higher levels.
This level does not indicate there is no protection at all, just that the entity doesn't qualify for the higher levels.

2 - Restricted:
: Entities at this level MUST meet was is described below in Restricted Characteristics.
Security at this level is aimed at defending against large-scale network/remote attacks by having a reduced attack surface.

3 - Hardware:
: Entities at this level are indicating they have some countermeasures to defend against physical or electrical attacks against the entity.
Security at this level is aimed at defending against attackers that physically capture the entity to attack it.
Examples include TPMs and Secure Elements.

The security level claimed should be for the weakest point in the entity, not the strongest.
For example, if attestation key is protected by hardware, but the rest of the attester is in a TEE, the claim must be for restriced.

This set of three is not extensible so this remains broadly interoperable. In particular use cases, alternate claims may be defined that give finer grained information than this claim.

See also the DLOAs claim in EAT {{dloas}}, a claim that specifically provides information about certifications received.


## Restricted Characteristics
* The entity's security configuration must be controlled by the vendor of the commercial device or its delegates or its suppliers.
* The entity must protect itself from modifications degrading its security. This includes modifications when powered-off. It hence requires a secure boot process of the entity.
* The entity must provide full isolation from any rich OS or external devices or operating environments it connects with except for conveyance of protocol messages intended for communication with the rich OS and external devices or operating environments. As a consequence, it must not be possible for SW or HW on the same device but outside the entity to modify any state, registers, memory or storage inside the operating environment.
* The entity should be security-oriented with the bulk of the functionality it hosts and provides being focused primarily on security (e.g., not large graphics engines, signal processors, general purpose app hosting, network stacks and such).
* The apps hosted by the entity should be primarily security-oriented (e.g., does not host thousands of downloadable games, complex productivity apps like word processors, or large scale network apps like web browsers).
* A security oriented SW engineering practice should be followed
    * Code is reviewed by security experts
    * A security patch system is in place
    * Security incidents are tracked
    * Security coding practice is followed
    * System documentation is produced


## CDDL

    $$Claims-Set-Claims //=
        ( security-level-label => security-level-type ) 
    
    security-level-type = unrestricted /
                          restricted /
                          hardware

    unrestricted       = JC< "unrestricted",      1>
    restricted         = JC< "restricted",        2>
    hardware           = JC< "hardware",          3>


## Why Security-Level is not based on Certification

The simple answer is that certification is very expensive.
The goal here is to provide an alternative that is not expensive.
Also, there is a different claim, DLOAs, for certification.

