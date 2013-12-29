swchacha
========

Verilog 2001 implementation of the ChaCha stream cipher.

## Purpose ###
The main purpose of this implementation is as a proof of concept and as
a comparison of hardware complexity and performance in comparison with
Salsa20 and other ciphers.


## Functionality ##
This core implements ChaCha with support for 128 and 256 bit keys. The
number of rounds can be set from two to 32 rounds in steps of two. The
default number of rounds is eight.

The core contains an internal 64-bit block counter that is automatically
updated for each data block.


## Performance ##
Each quarterround takes one cycle which means that the mininum latency
will be 4*rounds. When the core is functionally correct we will add two
more version with 2 and 4 parallel quarterrounds respectively. The four
quarterounds version will achieve 1 cycle/round.


## Implementation ##
Implementation results using the Altera Quartus 13 design tool.

### Cyclone IV GX ###
- 6233 LEs
- 3677 registers
- 56.1 MHz
- 11 cycles latency
- 2.6 Gbps performance.


### Cyclone V GX ###
- 2631 ALMs for logic
- 3677 registers
- 54.3 MHz
- 11 cycles latency
- 2.5 Gbps performance.


## Status ##
(2013-12-29)
- Changed to four parallel QR engines to more than quadruple the
performance using 5% more logic. The latency is down from  35 cycles (8
* 4 + 3 cycles) for 8 rounds to 11 cycles (2 * 4 + 3).

(2013-12-20)
- Top and core are verified. A somewhat major overhaul of the design was
needed to fix proper handling of multiple block messages.

- The design has been synthesized and senth trough the Altera Quartus
backend to get completed implementations for Alteras Cyclone IV GX and
Cyclone V GX devises.


(2013-12-17)
- Top is almost completely verified. Top testbench runs self checking
test cases.

(2013-12-10)
- Core is now functionally correct. Still need to be pushed through
backend and be cleaned up.
- Top is not functionally verified.


(2013-11-03)
- The core round processing is (seems to be) functionally correct.
- The Python model has been cleaned up. A few minor display bugs has
been fixed.


(2013-10-13)
- Python model now completed and pass all tests cases generated by the
  reference model. 


(2013-10-06)
- Added TC1 from chacha_testvectors. Testing shows that the Python model
generates correct result, but is intra word-big endian, not little
endian. We need to flip the words around.



(2013-10-04)
- The reference model has been moved to a separate project created just
  to generate and document ChaCha test vectors. See:
  https://github.com/secworks/chacha_testvectors


(2013-10-03)
- There is now also a c reference model that is intended to generate
test vectors for different combinations of keys, iv, blocks and rounds.


(2013-09-26)
- Debugging of the core is ongoing with quarterround at the focus.
- The core goes through the motions of processing blocks.
- There is a testbench for the top level the top seems to work ok.
- There is a Python model being developed in src/model.
- Quarterround in the Python model works. Used to debug the RTL.


(Older stuff)
- The current implementation consists of a ChaCha core with (very) wide
  data interfaces. 

- There is also a top level wrapper, chacha.v that provides a memory
  like 32-bit API. Note that with

- The core is currently not completed. The datapath and control is
  basically completed but needs to be debugged. The initial version of
  the testbench for the core is done but the core is not yet connected.

- The top level wrapper is functionally completed, but not yet
  debugged. There is no testbench.


## Notes ##

