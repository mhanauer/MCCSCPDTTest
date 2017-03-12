# MCCSCPDTTest
# Example for Jags-Ymet-Xnom2grp-MrobustHet.R 
#------------------------------------------------------------------------------- 
# Optional generic preliminaries:
graphics.off() # This closes all of R's graphics windows.
rm(list=ls())  # Careful! This clears all of R's memory!
#------------------------------------------------------------------------------- 
# Load The data file 

setwd("/Users/matthewhanauer/Desktop/DBDA2Eprograms")
myDataFrame = read.csv( file="mccsc5.csv" )
xName="mccsc3"
yName="mccsc2"
fileNameRoot="mccsc5-"
RopeMuDiff=c(-0.1,0.1) ; RopeSdDiff=c(-0.1,0.1) ; RopeEff=c(-0.2,0.2)


graphFileType = "eps" 
#------------------------------------------------------------------------------- 
# Load the relevant model into R's working memory:
source("Jags-Ymet-Xnom2grp-MrobustHet.R")
#------------------------------------------------------------------------------- 
# Generate the MCMC chain:
mcmcCoda = genMCMC( datFrm=myDataFrame , yName=yName , xName=xName ,
                    numSavedSteps=50000 , saveName=fileNameRoot )
#------------------------------------------------------------------------------- 
# Display diagnostics of chain, for specified parameters:
parameterNames = varnames(mcmcCoda) # get all parameter names
for ( parName in parameterNames ) {
  diagMCMC( codaObject=mcmcCoda , parName=parName , 
            saveName=fileNameRoot , saveType=graphFileType )
}
#------------------------------------------------------------------------------- 
# Get summary statistics of chain:
summaryInfo = smryMCMC( mcmcCoda , RopeMuDiff=RopeMuDiff , 
                        RopeSdDiff=RopeSdDiff , RopeEff=RopeEff ,
                        saveName=fileNameRoot )
show(summaryInfo)
# Display posterior information:
plotMCMC( mcmcCoda , datFrm=myDataFrame , yName=yName , xName=xName , 
          RopeMuDiff=RopeMuDiff , RopeSdDiff=RopeSdDiff , RopeEff=RopeEff ,
          pairsPlot=TRUE , saveName=fileNameRoot , saveType=graphFileType )
#-------------------------------------------------------------------------------
