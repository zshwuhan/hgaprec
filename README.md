Installation
------------

Required libraries: gsl, gslblas, pthread

On Linux/Unix run

 ./configure
 make; make install

On Mac OS, the location of the required gsl, gslblas and pthread
libraries may need to be specified:

 ./configure LDFLAGS="-L/opt/local/lib" CPPFLAGS="-I/opt/local/include"
 make; make install

The binary 'gaprec' will be installed in /usr/local/bin unless a
different prefix is provided to configure. (See INSTALL.)

HGAPREC: Hierarchical Gamma Poisson factorization based recommendation tool
----------------------------------------------------------------------------

**hgaprec** [OPTIONS]

   -dir <string>    path to dataset directory with 3 files:
   		    train.tsv, test.tsv, validation.tsv
		    (for examples, see example/movielens-1m)
 
   -m <int>	  number of items
   -n <int>	  number of users
   -k <int>	  number of factors
   
   -rfreq <int>	  assess convergence and compute other stats 
   		  <int> number of iterations
		  default: 10

   -a
   -b		  set hyperparameters
   -c		  default: a = b = c = d = 0.3
   -d
   
   
   -hier	  learn the hierarchical model with Gamma priors
                  on user and item scale parameters

   -bias	  use user and item bias terms
   
   -binary-data	  treat observed data as binary
   		  (if rating > 0 then rating is treated as 1)
   		  
   -gen-ranking	  generate ranking file to use in precision 
   		  computation; see example		  

   -msr		  write out ranking file assuming the test file
                  is based on leave-one-out, i.e., leaving one
                  item out for each user

Example
--------

1. Input data

   To run inference you need 4 files: {train,test,validation,test_users}.tsv in tab-separated format.
   See the movielens files in example/

   (Additional files are generated from these basic files during evaluation. More on this later.)

2. Running the command

   You can run "hgaprec" directly or using the scripts/run.pl Perl script.

   Since hgaprec has numerous options, the perl script is recommended.

   To setup the Perl script, do the following:

   a. Set the "$dataloc" variable to the location of the data set directory. For example, for the movielens data, 
   $dataloc = "/n/fs/example/movielens";

   b. Set the location of the installed hgaprec binary. For example,
   $gapbin = "/n/fs/bin/hgaprec";

   c. Run the script in one of the following ways:

   (fit the hierarchical model, i.e., HPF)
   <path-to>/scripts/run.pl -dataset movielens -hier 

   (fit HPF to data treated as binary)
   <path-to>/scripts/run.pl -dataset movielens -hier -binary

   (fit the non-hierarchical model, i.e., BPF to data and treat data as binary)
   <path-to>/scripts/run.pl -dataset movielens -binary

   (fit the non-hierarchical model with user, item bias terms, i.e., BPF+bias to data)
   <path-to>/scripts/run.pl -dataset movielens -bias

   (fit the non-hierarchical model with user, item bias terms, i.e., BPF+bias to data treated as binary)
   <path-to>/scripts/run.pl -dataset movielens -bias -binary


  d. Additional options to the Perl script

  -K <integer>: set the latent dimensions K
  -label <string>: set a label for the output directory
  -seed <integer>: set the pseudo-random number generator seed 
  -logl: compute ELBO every X iterations (expensive!)